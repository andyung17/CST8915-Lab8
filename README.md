# CST8915 Lab 7: Introduction to Kubernetes Basics

**Student Name**: Andy Ung <br/>
**Student ID**: 041299387 <br/>
**Course**: CST8915 Full-stack Cloud-native Development <br/>
**Semester**: Fall 2026 <br/>

---

## Demo Video

🎥 [Watch Demo Video](https://youtu.be/rlH7Djphdcs)


---

## Analysis

- Whether RabbitMQ is a stateless or stateful application?

RabbitMQ is a stateful application and should use a persistent volume. Message queues, payloads, etc are stored on the RabbitMQ instance.

- The implications of running RabbitMQ without persistent storage?

Without persistent storage, after a pod that contained a `services` of RabbitMQ, the orders that are queue'd are destroyed and lost. This means creating a new service of RabbitMQ does not retain the same messages

- What happens when the RabbitMQ pod is deleted or restarted?

In this current `yaml` file, when a RabbitMQ pod is deleted or restarted, all orders are lost. This is due to the message queues are stored on the rabbitmq instance pod so whenever the pod is restarted or deleted, the state is no longer retained.

- Potential solutions to this problem (research-based)?

A possible solution is using persistent storage that is linked to a RabbitMQ service. This means whenever a RabbitMQ pod is newly created, restarted, or destoryed, all orders are stored to serve to a client.

Below is an example of adding a persistent storage to maintain statefulness. 
- `volumeMounts` specifies the directory to store the message queues, etc
- `volumes` tells the pod how to source the orders. In this case a cloud disk storage is provisioned
```
spec:
  template:
    spec:
      containers:
        - name: rabbitmq
          image: rabbitmq:3-management

          # Extension
          volumeMounts:
            - name: rabbitmq-storage
              mountPath: /var/lib/rabbitmq

      # Extension
      volumes:
        - name: rabbitmq-storage
          persistentVolumeClaim:
            claimName: rabbitmq-pvc
```

- Does using Azure Service Bus solve the issues identified with RabbitMQ Configuration in this Lab?

Yes, using a managed service instead of an owned service moves the responsibility to Azure/Microsoft to manage the infrastructure and provisioning (and more..). This means that we do not need to worry about the issues of orders/messages being lost after a queue is restarted or destroyed.  

## Acknowledgements
- https://www.rabbitmq.com/docs/install-kubernetes-diy
- https://www.rabbitmq.com/docs/install-kubernetes-diy
- Had AI help create the sample extension part to the YAML
