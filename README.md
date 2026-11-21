# CST8915 Lab 8: Deploying and Managing the Algonquin Pet Store (On Steroids)

**Student Name**: Andy Ung <br/>
**Student ID**: 041299387 <br/>
**Course**: CST8915 Full-stack Cloud-native Development <br/>
**Semester**: Fall 2026 <br/>

---

## Demo Video

🎥 [Watch Demo Video](https://youtu.be/yWT4jvTXNvE)


---

## Updated Files

[Admin Task](./admin-tasks.yaml) <br/>
[Aps All in One Task 1](./aps-all-in-one-Task1.yaml)

---

## Investigate Azure Managed Services
Azure CosmosDB
- Replaces: MongoDB
- Purpose: Fully managed noSQL database
- MongoDB is a good fit as its a fully managed noSQL db that stores data
- MongoDB was used by the `makeline-service` to store persistent orders, but if the MongoDB pod restarts or fails, the orders are lost
- CosmosDB is a good fit as it supports MongoDB wire protocol and should only need a connection string instead of fully rewriting code
- This solves the problem of losing data on a pod restart, since the service stores data outside the pod and handles patching and scaling. This allows us to focus on the application instead of managing the database.

Azure Service Bus
- Replaces: RabbitMQ
- Purpose: Fully managed message broker with queues and topics
- Azure service bus is a good fit as it is a fully managed service that acts as a message broker with queues and topics and does not lose data if it is temporarily down.
- RabbitMQ was used to queue orders from `order-service` and `makeline-service` and using Azure service bus does the same job with AMQP to queue orders
- The problem this solves is we no longer have to run and maintain the broker ourselves
- Orders are continually fed to the queue if a broker pod fails instead of lost messages when our RabbitMQ pod goes down or restarts

## Acknowledgements
- Had AI help modify the all in one YAML
- https://learn.microsoft.com/en-us/azure/cosmos-db/mongodb/overview
- https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview
