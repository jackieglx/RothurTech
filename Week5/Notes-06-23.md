# `@Transactional` 的边界：本地事务不等于分布式事务

@Transactional 默认解决的是 local transaction。

同一个事务管理器控制的数据库操作：
全部成功，或者全部回滚。

跨微服务后：
不同 JVM
不同数据库
不同网络请求
不同失败时间点

普通 @Transactional 无法自动保证整个业务流程的原子性。



## 需要进一步掌握

- `@Transactional` 为什么通过 AOP proxy 实现
- transaction boundary 通常为什么放在 Service 层
- self-invocation 为什么可能导致事务失效
- propagation
- isolation level
- rollback rule
- local transaction 和 distributed transaction 的区别
- 数据库事务成功，但消息发送失败怎么办



# 分布式事务方案：Two-Phase Commit，2PC

```
Phase 1: Prepare

Coordinator 问所有 participant：
你是否已经准备好提交事务？

Service A: Yes
Service B: Yes
Service C: Yes

Phase 2: Commit / Rollback

全部 Yes：
Coordinator 通知所有 participant commit

只要有一个 No：
Coordinator 通知所有 participant rollback
```

## 2PC的主要问题

2PC（Two-Phase Commit）的主要问题可以总结为：

### 1. 延迟高

一次事务需要经过两个阶段：

1. Prepare：询问所有服务是否准备好
2. Commit/Rollback：通知所有服务提交或回滚

因此需要多轮网络通信。任何一个服务响应慢，整个事务都会被拖慢。

### 2. 阻塞问题

参与者在 Prepare 成功后，必须等待 Coordinator 的最终决定。

等待期间，数据库事务可能一直保持打开，并占用：

- 数据库连接
- 行锁或表锁
- 事务日志
- 其他资源

因此并发量高时，容易影响数据库吞吐量。

### 3. Coordinator 是关键故障点

如果 Coordinator 挂掉，参与者可能不知道应该提交还是回滚，事务会进入 **in-doubt state（不确定状态）**。

即使所有业务服务都正常，分布式事务也可能无法继续执行。

可以通过部署多个 Coordinator 提高可用性，但这又会增加状态同步、选主和故障恢复的复杂度。

### 4. 一个慢服务拖累所有服务

2PC 必须等待所有参与者都 Prepare 成功。

例如：

```
Service A: ready
Service B: ready
Service C: response is very slow
```

整个事务都必须等待 Service C，因此系统性能由最慢的参与者决定。



# 分布式事务方案：Saga

锁住整个事务，使用补偿操作恢复业务

Saga 的思路不是让所有服务同时 commit，而是：

```
Service 1 先提交自己的本地事务
→ Service 2 提交
→ Service 3 提交

如果 Service 3 失败：
执行 Service 2 的补偿操作
再执行 Service 1 的补偿操作
```

## Saga 的两种形式：Choreography 和 Orchestration

### 1. Choreography：事件编舞

没有统一的流程控制器。

每个服务：

1. 完成本地事务
2. 发布事件
3. 下一个服务监听事件
4. 继续处理



它常见的实现更像：

```
OrderCreated
    ↓
Inventory Service consumes
    ↓
InventoryReserved
    ↓
Payment Service consumes
    ↓
PaymentCompleted
```

可以使用：

- Kafka
- RabbitMQ
- SNS/SQS
- 其他 event broker

优点

- 服务相对解耦
- 不依赖中心流程服务
- 易于异步扩展

缺点

- 整个业务流程不直观
- 不容易看出现在执行到哪一步
- debugging 困难
- 事件链很长后容易形成 event spaghetti

### Orchestration：集中编排

引入一个 orchestrator，明确控制：

```
先调用 Order Service
成功后调用 Inventory Service
成功后调用 Payment Service

Payment 失败：
调用 Inventory compensation
调用 Order compensation
```

优点

- 整个 workflow 清楚
- 容易追踪 Saga 状态
- 业务流程修改集中
- 错误处理和补偿更明确

缺点

- orchestrator 逻辑可能越来越复杂
- 容易与多个服务强耦合
- orchestrator 也需要集群、高可用和持久化状态

注意：

> 逻辑上集中，不代表部署上只能有一个实例。

Orchestrator 可以部署多个实例，通过数据库或状态存储共享 Saga 状态。



# 多云和容灾：不要把所有风险押在一个平台上

```
99% 流量运行在内部 Apple cloud infrastructure
1% 流量长期运行在 AWS、GCP、Azure standby instances

主平台故障后：
快速 scale out 公有云实例
接管 100% 流量
```