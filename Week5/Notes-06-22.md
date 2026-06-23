

```
单体应用
   ↓ 按业务领域拆分
多个微服务
   ↓ 单个服务承受不了流量
多个实例 / 横向扩容
   ↓ 实例会动态上下线
服务发现
   ↓ 多个健康实例中需要选择一个
负载均衡
   ↓ 配置不能散落在几十个实例中
配置中心 + Secret Manager / Vault
   ↓ 需要知道系统是否健康
监控、日志和告警
   ↓ 数据库容易成为瓶颈
本地缓存 + Redis
   ↓ 下游服务可能失败
Timeout + Retry + Circuit Breaker + Fallback
   ↓ 服务越多，网络与一致性成本越高
根据业务需求做取舍，而不是迷信微服务
```



# 配置中心和 Vault 的区别

- 普通配置集中放入 Configuration Server
- 按 `dev / QA / UAT / prod` 区分环境
- 密码、数据库凭证等敏感数据不应该直接放在配置中心
- 敏感信息应该放在 Vault 一类的 Secret Manager 中



# 监控不是只看图，而是形成告警闭环

```
Spring Boot Actuator
        ↓
Prometheus 收集 metrics
        ↓
Grafana 展示 dashboard
        ↓
达到阈值
        ↓
Slack / Teams 告警
        ↓
工程师看日志、复现、定位根因
```



# 二级缓存和冷缓存问题

```
Request
   ↓
L1：应用实例内部的 Local Cache
   ↓ miss
L2：集中式 Redis
   ↓ miss
Database
```



# 重要请求临时持久化

重要请求临时持久化，就是先把尚未完成的业务请求可靠记录下来，使它不会因为服务宕机、网络故障或进程重启而丢失，然后通过异步处理、重试和补偿完成业务。



# 失败处理

```
Service A 调用 Service B
Service B 已经故障
        ↓
A 中大量请求等待超时
        ↓
线程、连接、内存被占用
        ↓
A 也可能被拖垮
        ↓
级联故障
```

然后使用：

```
有限次数 Retry
       ↓ 仍然失败
Circuit Breaker Open
       ↓
快速失败
       ↓
Fallback / 降级响应
```



Retry 必须有限次数。

- 应采用 backoff，通常还要有 jitter。
- 不是所有异常都应该重试。
- 写操作必须考虑幂等。
- 大量重试可能形成 retry storm，把正在恢复的服务再次压垮。



# 不是所有情况都适合微服务架构

Prime Video 在 2023 年公开的 **Video Quality Analysis（VQA，视频质量分析）监控服务**案例

Prime Video 的一个 Video Quality Analysis 服务负责实时检测视频冻结、画面损坏和音画不同步等问题。

最初，这个系统使用 AWS Step Functions、Lambda 和 S3，把视频转换、缺陷检测和结果聚合拆成多个分布式组件。

视频帧需要先上传到 S3，再由多个检测服务下载处理，因此产生了大量网络传输和 S3 API 调用。

同时，每秒视频都需要触发多次 Step Functions 状态转换，导致成本和扩展压力快速增加。

这套架构在只达到目标负载约 5% 时就遇到了明显的扩展和成本问题。

后来，团队把 Media Converter、Detectors 和 Aggregation 合并到同一个 ECS Task 和进程中，通过内存直接传递视频帧。

新设计仍然可以通过运行多个 ECS Task 横向扩容，但减少了不必要的远程调用、中间存储和编排开销。

- 在 Prime Video 的案例里，**一个 ECS Task 可以包含 Media Converter、多个 Detector 和 Result Aggregator**。这些组件运行在同一个 Task 或进程里，所以可以直接通过内存传递视频帧，而不必经过 S3 和大量远程调用。
- ECS：AWS 自己的容器编排系统，比较简单，和 AWS 集成紧密
- EKS：AWS 托管的 Kubernetes，功能更通用，但更复杂

最终基础设施成本降低了 90% 以上，这个案例说明紧密耦合、高频通信的组件不一定适合拆成细粒度微服务。