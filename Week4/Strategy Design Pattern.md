# what is Strategy Pattern 

The Strategy Design Pattern puts different algorithms or business rules into separate strategy classes. These strategies solve the same problem in different ways, so they can replace each other. At runtime, the program chooses the most suitable strategy based on the current condition.



**Strategy Design Pattern（策略模式）**是把一组可以互相替换的算法或业务规则，分别封装成不同的策略类，运行时根据条件选择其中一个执行

因为这些策略解决的是同一个问题，只是实现方式不同，所以可以互相替换。



# why Strategy Pattern 

The Strategy Pattern puts different algorithms or business rules into separate implementation classes. It helps reduce complex `if-else` or `switch` statements.

When we need a new strategy, we usually only need to add a new implementation class without changing the existing core logic.

The program can also switch between different strategies at runtime, which makes the system more flexible and easier to extend.



策略模式可以把不同的算法或业务规则封装成独立的实现类。它能够减少大量复杂的 `if-else` 或 `switch` 判断。

新增策略时通常只需要增加新的实现类，不必修改原有核心代码。

不同策略可以在运行时灵活切换，提高系统的扩展性。



# how do you implment in your project

A common use case is an order system that supports different payment methods, such as credit card, PayPal, and Apple Pay.

一个常见应用场景是：**订单系统支持不同的支付方式**，例如信用卡、PayPal 和 Apple Pay。

## 1. 定义策略接口

```java
public interface PaymentStrategy {

    void pay(double amount);
}
```

## 2. 实现不同策略

```java
public class CreditCardPayment implements PaymentStrategy {

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " by credit card.");
    }
}
public class PayPalPayment implements PaymentStrategy {

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " by PayPal.");
    }
}
public class ApplePayPayment implements PaymentStrategy {

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " by Apple Pay.");
    }
}
```

## 3. 创建 Context 类

`PaymentService` 不关心具体使用哪种支付方式，它只调用统一的策略接口。

```java
public class PaymentService {

    private PaymentStrategy paymentStrategy;

    public PaymentService(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void processPayment(double amount) {
        paymentStrategy.pay(amount);
    }
}
```

## 4. 运行时选择策略

```java
public class Main {

    public static void main(String[] args) {
        PaymentService paymentService =
                new PaymentService(new CreditCardPayment());

        paymentService.processPayment(100);

        // 运行时切换策略
        paymentService.setPaymentStrategy(new PayPalPayment());
        paymentService.processPayment(200);

        paymentService.setPaymentStrategy(new ApplePayPayment());
        paymentService.processPayment(300);
    }
}
```