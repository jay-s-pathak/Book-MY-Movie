# Book-MY-Movie

Book-MY-Movie is a web interface for booking movies. 
It uses various microservices to handle different requests.

# Architecture

![Architecture](Architecture.png)


# Design Summary

### User Layer
The user layer handles the client side of the system. This system is deployed on Google Cloud Platform. It uses React framework for frontend and makes api calls.

### API Gateway layer
Kong API Gateway handles the requests coming from the user interface and it works in DBless mode. It is responsible for routing and calling respective microservices. It is deployed on Google Cloud Platform.

### API layer
The api layer has 5 seperate microservices which are implemented as follows

#### 1. TheaterAPI
Used for Theater registration and deregistration from booking system.
#### 2. MovieAPI
Used for adding new/active movies and removing existing/inactive movies from the system.   
#### 3. UserAPI
Used for registartion and deregistration of users in booking system. 
#### 4. MailAPI
Used for sending acknoledgement of booking confirmations via Email and Text.
#### 5. ShowAPI 
Used for managing number of seats and users in a particular show while booking.

Each microservice has its individual classic laodbalancer connected to an autoscaling group. 

### Database Layer
Each microservice uses a sharded mongoDB cluster. Each cluster contains following members:

- 1 Query Router
- 2 Config Servers
- 2 Shards each having a Replica Set which consists of 3 mongoDB instances
# AKF Scaling Cube:
### X-axis Scaling
To achieve x-axis scaling we have dockerized GO API and deployed it to EC2 instance which runs behind a Load Balancer on AWS.

### Y-axis  Scaling
To achieve y-axis scaling we have used Service Oriented Architecture by dividing business logic into different microservices namely :
- UserAPI
- TheaterAPI
- MovieAPI
- ShowAPI
- MailAPI

### Z-axis Scaling
To achieve z-axis scaling we have used MongoDB sharded cluster which has been deployed on AWS. MongoDB sharded cluster contains 1 Mongo Query Router, 2 config servers, 2 sharded replica sets.


