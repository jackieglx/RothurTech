# S3 link

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-03-21-18-10.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260604%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260604T043456Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=7758deb84be66aaf8c4ed4415ad32823f940a0debc8eadf7c477188653f89ce1



## 1. Client-server model

The client-server model means the client sends a request, and the server processes the request and returns a response.

The client can be a web browser, a mobile app, or another service. The server usually runs the business logic, talks to the database, and returns data to the client.

For example, when a user opens an e-commerce website and searches for a product, the browser sends a request to the backend server. The server queries the product data and sends the result back to the browser.

------

## 2. Application service

An application service is a backend service that handles a specific part of the business logic.

For example, in an e-commerce system, we may have a product service, payment service, order service, and shipment service. Each service is responsible for one business capability.

In Spring Boot, we usually organize the logic into controller layer, service layer, and DAO layer. The controller receives and validates the request, the service layer handles business logic, and the DAO layer communicates with the database. 

------

## 3. HTTP request / response

HTTP request and response are the common way for the client and server to communicate.

An HTTP request usually contains the HTTP method, URL, headers, and sometimes a request body. The method tells the server what operation the client wants to perform. For example, GET is used to read data, POST is used to create data, PUT is used to update the whole resource, PATCH is used to update part of the resource, and DELETE is used to delete data.

An HTTP response usually contains a status code, headers, and response body. For example, 200 means success, 400-level errors usually mean client-side problems, and 500-level errors usually mean server-side problems.

------

## 4. Horizontal scaling vs vertical scaling

Vertical scaling means improving the capacity of one server, such as adding more CPU, memory, or disk.

Horizontal scaling means adding more server instances and distributing traffic across them.

Vertical scaling is simpler at first, but it has a hardware limit. Horizontal scaling is usually more flexible because we can add more instances during high traffic and remove instances when traffic goes down. But horizontal scaling also requires more system design work, such as load balancing, service discovery, and data consistency handling.



------

## 5. Load balancer

A load balancer distributes incoming traffic across multiple service instances.

We need it because one instance may not be able to handle all requests. If we have multiple instances of the same service, the load balancer can send requests to different instances, so the workload is more balanced and the latency is lower.

For example, if the product service has five instances, the load balancer can route requests to these instances instead of sending all traffic to one server.

------

## 6. Microservice

Microservice architecture means splitting a large application into smaller independent services based on business scope.

For example, in an e-commerce system, product, payment, order, and shipment can be separate services. Each service can be developed, deployed, and scaled independently.

We need microservices because they improve availability, scalability, and fault isolation. If the shipment service is down, users may still browse products and make payments. But the trade-off is that services now need to communicate over the network, so we need to handle latency, failures, data consistency, and monitoring.

------

## 7. Microfrontend

Microfrontend means splitting a large frontend application into smaller frontend modules, usually based on business domains or teams.

It is similar to microservices, but it applies to the frontend. For example, in an e-commerce website, product pages, checkout pages, and user profile pages may be owned by different teams and deployed independently.

We need it when the frontend becomes large and multiple teams need to work independently. The trade-off is that it adds complexity in routing, shared UI components, state management, and consistency of user experience.

------

## 8. Relational database / SQL database

A relational database stores data in tables with rows and columns. It usually uses SQL to query and manage data.

We use relational databases when the data has a clear structure and we need strong consistency, transactions, and relationships between tables.

For example, in an e-commerce system, orders, users, payments, and order items can be stored in relational tables. MySQL, PostgreSQL, and Oracle are common relational databases.

------

## 9. Nonrelational database / NoSQL database

A nonrelational database, or NoSQL database, stores data in a more flexible format, such as key-value, document, wide-column, or graph.

We use NoSQL when we need flexible schema, high scalability, or very high read/write throughput.

For example, Redis can be used as a key-value cache, MongoDB can store JSON-like documents, and DynamoDB can handle large-scale key-value or document workloads.

## 10. API gateway

An API gateway is the entry point of the backend system.

It receives client requests and routes them to the correct backend service. It can also handle cross-cutting concerns such as authentication, authorization, rate limiting, logging, and sometimes load balancing.

For example, if the client requests product data, the API gateway routes the request to the product service. If the client wants to check shipment status, it routes the request to the shipment service.

------

## 11. Message queue

A message queue is used for asynchronous communication between services.

Instead of calling another service directly and waiting for the result, one service can send a message to the queue, and another service can consume it later.

We need message queues to decouple services, handle traffic spikes, improve reliability, and support asynchronous processing.

For example, after an order is created, the order service can send an order-created message to a queue. Then the payment service, inventory service, or notification service can process it asynchronously.

------

## 12. Log and monitor

Logs are records of what happened in the system. Monitoring is used to observe the health and performance of the system.

We need logs to debug problems and understand application behavior. We need monitoring to track metrics such as CPU usage, memory usage, latency, error rate, request count, and HTTP status codes.

For example, if users say they paid but did not receive a shipment email, we can check logs and monitoring data to see whether the payment service, message queue, or shipment service failed.

------

## 13. Deployment with AWS / Azure / GCP

Deployment means putting our application into an environment where users can access it.

With cloud platforms like AWS, Azure, or GCP, we do not need to manage all physical servers ourselves. We can use cloud services to run applications, store data, route traffic, monitor the system, and scale resources.

For example, on AWS, we can deploy backend services on EC2, ECS, or EKS. We can store files in S3, use RDS for relational databases, use SQS as a message queue, and use CloudWatch for logs and monitoring.

------

## 14. Security: authentication and authorization



Authentication means verifying who the user is. Authorization means checking what the user is allowed to do.

For example, login with username and password is authentication. After login, checking whether this user can access admin APIs is authorization.

In backend systems, we often use session-based authentication or token-based authentication like JWT. After the user logs in, the server issues a token. For later requests, the client sends the token, and the backend validates it before processing the request.

Authentication answers “Who are you?” Authorization answers “What are you allowed to access?”

------

## 15. Why testing?

We need testing to make sure the application works correctly, reduce bugs, and make future changes safer.

Unit tests verify small pieces of logic, such as one method or one class. Integration tests verify whether different components work together, such as service layer with database or message queue. End-to-end tests verify the whole user flow from the client side to the backend.

Testing is important because modern applications change frequently. Without tests, every code change may break existing features. With tests, we can catch problems earlier before deploying to production.