David 关于周末作业 **“RESTful endpoint design from scratch”** 的原话是：

> “For the homework for this weekend, do a RESTful endpoint coding exercise. We have already done that application as homework. Right now, practice the controller-layer RESTful endpoint design from scratch.”

> “Your controller layer contains at least four CRUD operations: read, insert, update, and delete.”

> “You should be able to design and implement it from your IDE from scratch. It starts from creating a Spring Boot application, then declaring the business requirement, and then designing your RESTful endpoints.”

> “A lot of you guys don’t use the request header, request parameter, or path variables. Use those things to design your RESTful endpoints.”

有人问业务场景是不是必须继续使用 Student Management System，David 回答：

> “It could be anything—employee, student, account, transaction. It’s actually the same thing. It’s just about four RESTful endpoints for the CRUD operations.”

他还强调 response：

> “Always remember to wrap up your payload with `ResponseEntity`. This is a standard JSON format that you’re going to return back to the UI. With `ResponseEntity`, you have the payload and HTTP status code.”

关于需要达到的熟练程度，他说：

> “You are going to do this from scratch. If you are given an IDE, you should be able to create a Spring Boot application and then design the controller layer. If you are given a whiteboard, you should be able to write all of those codes. You don’t have to import any libraries, but you should be able to talk about them, understand them, and write the code.”

关于 recording，他说：

> “Practice it with your recording. Open your mouth while you’re coding; you’re also talking.”

> “I want a recording of that practice. Don’t do the practice in silence. It doesn’t help, because you might have a problem coding in 40 minutes to design a RESTful endpoint, including declaring the business requirements, coding, and talking about your ideas. That’s practical for the interview.”

  

所以作业要求可以概括为：

1. 从零创建一个 Spring Boot application。
2. 自己声明一个业务场景，employee、student、account 或 transaction 都可以。
3. Controller 至少实现四个 CRUD endpoints：read、insert、update、delete。
4. endpoint design 中要练习使用 request header、request parameter 和 path variable。
5. 使用 `ResponseEntity` 返回 payload 和 HTTP status code。
6. 全程录屏，并且边写代码边讲思路。
7. 目标是在大约 40 分钟内从业务需求讲到 endpoint design 和 coding。