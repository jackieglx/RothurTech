# TCP 3 way handshaking

First, the client sends a `SYN` packet to the server to request a TCP connection.

Second, after the server receives the client’s `SYN`, it replies with a `SYN-ACK`. The `ACK` part confirms that the server is able to receive the client’s request and the client is able to send request, so the client-to-server direction is working.

Third, after the client receives the server’s `SYN-ACK`, it sends back an `ACK`. This confirms that the client is able to receive the server’s response and the server is able to send the response, so the server-to-client direction is also working.

After these three steps, both sides know they can send and receive data, and the TCP connection is established.





# TCP vs UDP

In terms of connection, TCP is connection-oriented, so it needs a three-way handshake before sending data. UDP is connectionless, so it sends packets directly without establishing a connection.

In terms of reliability, TCP is reliable because it uses ACKs, retransmission, and sequence numbers to make sure data is delivered correctly. UDP does not guarantee delivery.

In terms of ordering, TCP guarantees that data is received in order, while UDP does not guarantee packet order.

In terms of performance, UDP is faster and more lightweight because it has less overhead. TCP is heavier because it provides reliability, flow control, and congestion control.

In terms of use cases, TCP is used when correctness is more important, such as HTTP, HTTPS, file transfer, and database connections. UDP is used when low latency is more important, such as DNS, video streaming, online gaming, and VoIP.



# what is Tomcat

Tomcat is a Java web server and Servlet container. It receives HTTP requests, passes them to the correct Servlet and sends HTTP responses back to the client. 

We need Tomcat because a normal Java class cannot directly receive HTTP requests

In Spring, Tomcat receives the HTTP request and passes it to Spring’s `DispatcherServlet`. `DispatcherServlet` routes the request to the correct Spring Controller method.

In Spring Boot, Tomcat is usually embedded, so when we start the Spring Boot application, Tomcat starts automatically and listens on a port like 8080.

So Tomcat is the runtime server that allows our Java web application to communicate with clients over HTTP.





# what are the basic components for tomcat

The basic components of Tomcat include `Server`, `Service`, `Connector`, `Engine`, `Host`, `Context`, and `Wrapper`. 

`Server` is the top-level component in Tomcat. It represents the whole Tomcat instance. Usually one Tomcat has one `Server`.

`Service` connects Connectors with the Engine. Usually, one `Service` has one `Engine`.

`Connector` listens on a port.

`Engine` receives requests from the Connector and decides which virtual `host` should handle the request.

`Host` represents a virtual host. One Tomcat can support multiple hosts.

A `Context` represents one web application deployed in Tomcat. 

A `Wrapper` represents one Servlet inside that web application.





# what is web server

A web server mainly handles HTTP or HTTPS traffic. It can serve static files, do reverse proxy, SSL termination, and load balancing. A web server usually **does not execute business code.** Examples are Nginx and Apache HTTP Server.  

An application server mainly runs backend application code, like Tomcat.

In real Java projects, Nginx is often placed in front to receive requests and forward dynamic API requests to Tomcat.

Nowadays, the boundary between web server and application server is a bit blurred. For example, Tomcat can also handle HTTP requests. But the key difference is whether the server is responsible for executing business code.



# what is 3 tire architecture

**3-tier architecture** means an application is divided into three layers: the **presentation layer**, the **business logic layer**, and the **data layer**. 

Presentation layer is the UI layer. It handles user interaction. Example: React, Angular, HTML/CSS, mobile app.

Business logic layer is the application layer. It handles business rules, validation, APIs, authentication, and coordination between the UI and database. Example: Spring Boot backend.

Data layer  stores and manages data. Example: MySQL, PostgreSQL, MongoDB, Redis.



# what is OSI Model, what is each layer doing

The OSI model is a conceptual model that explains how network communication works. It has seven layers: Physical, Data Link, Network, Transport, Session, Presentation, and Application. 

The Physical layer is responsible for sending raw bits over physical media.

The Data Link layer uses **MAC addresses** to deliver frames between devices in the same local network.

The Network layer uses **IP addresses** to route packets between different networks.

The Transport layer uses **port numbers** to provide end-to-end communication between processes. Common protocols are TCP and UDP.

The Session layer manages communication sessions between applications.

The Presentation layer handles data formatting, encoding, encryption, and compression.

The Application layer provides network services directly to applications, such as HTTP, DNS, SMTP, FTP, and SSH.