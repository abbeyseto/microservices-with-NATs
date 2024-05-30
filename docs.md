# Architecture V1 Description

This architecture represents a microservices-based system that utilizes an API Gateway for external access and NATS for inter-service communication. The key components and their interactions are described below:

Components

### API Gateway
Acts as the entry point for all client requests.
Routes requests to the appropriate microservices through NATS.
### NATS
A messaging system used for communication between the API Gateway and microservices, as well as between the microservices themselves.
Ensures decoupled and asynchronous communication.
### Microservices
#### User Service
Handles user-related operations (e.g., user creation, authentication).
Communicates with the Notification Service and accesses a shared PostgreSQL database.
#### Payment Service
Manages payment transactions and billing operations.
Interacts with the Notification Service and uses the shared PostgreSQL database.
#### Notification Service
Responsible for sending notifications (e.g., emails, SMS) based on events triggered by other services.
Listens to events from both the User and Payment services and stores data in the shared PostgreSQL database.
### PostgreSQL
A shared relational database used by all microservices for data persistence.
Each service has access to the database, allowing for consistent data storage and retrieval.

## Interactions

- Client to API Gateway: The client sends requests to the API Gateway, which routes the requests to the appropriate microservices via NATS.
- API Gateway to Microservices: The API Gateway sends messages to the User, Payment, and Notification services through NATS.

## Microservice to Microservice Communication:
- The User Service communicates with the Notification Service via NATS to send user-related notifications.
- The Payment Service communicates with the Notification Service via NATS to send payment-related notifications.
- The Notification Service listens to events from both the User and Payment services to trigger notifications.
- Database Access: Each microservice independently accesses the shared PostgreSQL database to read from and write to the appropriate tables.

## Flow

### User Registration:
- The client sends a user registration request to the API Gateway.
- The API Gateway routes the request to the User Service via NATS.
- The User Service processes the registration, saves user data in PostgreSQL, and sends a notification event to the Notification Service.
- The Notification Service sends a welcome notification to the new user.

### Payment Processing:
- The client sends a payment request to the API Gateway.
- The API Gateway routes the request to the Payment Service via NATS.
- The Payment Service processes the payment, updates the database, and sends a payment confirmation event to the Notification Service.
- The Notification Service sends a payment confirmation notification to the user.

This architecture ensures modularity, scalability, and flexibility, allowing individual services to evolve independently while maintaining communication through NATS. The shared PostgreSQL database provides a consistent data layer for all services.
