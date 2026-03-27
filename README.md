# Event-Driven E-Commerce Order Processing Engine (AWS)

A serverless event-driven backend that processes e-commerce orders using modern cloud architecture principles.
This project demonstrates how real-world companies design scalable backend systems where services communicate through **events instead of direct calls**, making the system **fault-tolerant, scalable, and loosely coupled**.

---

# Project Overview

In a traditional monolithic system, placing an order might directly call inventory, payment, and notification services in sequence. If any of those services fail or slow down, the entire order process fails.

This project solves that problem and redesigns the order processing system by implementing an **event-driven microservices architecture** where services react to events asynchronously.

When a customer places an order:

1. The order is received through an API.
2. The order is saved in a database.
3. An event is published announcing that an order was placed.
4. Independent services process the event in parallel:
   - Inventory service updates stock.
   - Notification service sends confirmation.

Because each service works independently, the system remains **resilient, scalable, and highly available**.

---

# Architecture Diagram


![Architecture Diagram](<Event-Driven E-Commerce Order Processing Engine Architecture Diagram.PNG>)


# Technologies Used


![AWS Technologies Used](<Screenshots/Aws Services Used.PNG>)


- AWS Lambda

- API Gateway

- DynamoDB

- SNS

- SQS

- CloudWatch

- Node.js


# System Workflow



**Step 1: Order Submission**

A client sends a **POST request** to the API endpoint.

**Step 2: Order Processing**

The Order Lambda function:

- Validates request data
- Generates an order ID
- Stores the order in DynamoDB
- Publishes an event to SNS

**Step 3: Event Distribution**

SNS broadcasts the order event to two SQS queues.

**Step 4: Asynchronous Processing**

Two independent services process the event:

**Inventory Service**
Reads the order details and processes stock updates.

**Notification Service**
Sends confirmation notifications to the customer.

**Step 5: Monitoring**

All services send logs to CloudWatch for monitoring and troubleshooting.

## Example Order Request

POST /orders

{

  "customerId": "cust-001",

  "items": [     
    {

      "productId": "prod-abc",

      "quantity": 2,

      "price": 29.99
    }
  ],

  "total": 59.98

}

## Example API Response 

{

  "orderId": "ord-1718035200000",

  "status": "PENDING",

  "message": "Order received and is being processed"

}


## Problems With Traditional Systems

Traditional tightly coupled systems face several limitations:

Problem	                              Impact

Service dependency         	         One failing service breaks the entire system

Poor scalability	                     All services scale together

Slow user response         	         Customers wait for all operations to complete

System instability         	         Traffic spikes overwhelm services


## Solutions Implemented in This Project

This architecture solves these issues by implementing:

**Event-Driven Architecture**

Services communicate through events rather than direct calls.

**Loose Coupling**

Each service operates independently and can be updated without affecting others.

**Horizontal Scalability**

Each Lambda function scales automatically based on demand.

**Asynchronous Processing**

Background tasks such as inventory updates and notifications run independently.

## Monitoring and Observability

Logs for all services are automatically stored in CloudWatch.

Log groups created:

/aws/lambda/OrderFunction
/aws/lambda/InventoryFunction
/aws/lambda/NotificationFunction

These logs help engineers monitor system behavior and troubleshoot issues.


## Future Improvements

This project can be expanded to simulate a full production-grade system.

Possible enhancements include:

**Payment Processing Service**

Add a payment microservice triggered by the order event.

**Email Notifications**

Use Amazon SES to send real email confirmations.

**Dead Letter Queues**

Capture failed messages for debugging and retry processing.

**Infrastructure as Code**

Deploy the entire system using Terraform or CloudFormation.

**CI/CD Pipeline**

Automate deployment with GitHub Actions.

**Order Status Updates**

Track order lifecycle states such as:

PENDING → PROCESSING → COMPLETED


