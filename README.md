# Starbucks Cashier Simulator
Starbucks Cashier Simulator simulates a real-life cashier system. The purpose of this project was to:
- Implement an application system end-to-end while also prioritizing scalability
- Work with a diverse tech stack

## Stack:
- Spring Boot: Supports MVC architecture and has a built-in IoC container
- MySQL (Docker Container): Store incomplete and completed orders, users, and user sessions
- KongAPI + HAProxy: Supports load balancing + API management
- RabbitMQ: Simulate a barista awaiting work (asynchronous, in a FIFO fashion)
- Docker: Promotes scalability by separating responsibilities of other services in their own containers
- Google Kubernetes Engine: Used to deploy application as an externally load-balanced ingress
- Asta: Provides an overview of the system design and making use of external/internal ports

## System Design
<img width="1175" alt="image" src="https://github.com/reddesignsguy/starbucks-project/assets/49921782/0223d76d-8290-4f3e-ab08-7f1e22c406db">

## Project Journal
### Day 1:

Today, I mostly focused on understanding how to tackle the project by taking notes and watching all the demos.

My first goal is to set up deployment to Docker and GKE. 

1. Set up the MySQL DB in Docker.
a. Run MySQL container
b. Run starbucks-api container
c. What connects them is application.properties from the starbucks-api container, which specifies the host, port, and database schema to be used from the MySQL container. Also, the username and password must be specified in app.props in order to use the database.

Created the DB.
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/49921782/236121574-41cb0282-941b-4d70-b25f-36af0ffc3d51.png">

### Day 2: 

Tested communication between Kong and the starbucks-api, but on the starbucks network. Had to edit commands to make docker deployment work.
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/49921782/236349979-f2dce7b9-f109-46dc-9872-6d0e2757cd63.png">

Testing the LB/starbucks service through Jumpbox for troubleshooting.
<img width="1512" alt="Screenshot 2023-05-04 at 10 39 31 PM" src="https://user-images.githubusercontent.com/49921782/236384507-a03981a5-2539-4f17-ac34-538149c80bf6.png">

Client successfully talks to Kong internally. The problem was, although I was able to reach Kong internally, Kong had not been properly configured to talk to the API-service. 

1. The kong.yaml file expected to be talking to a service named "starbucks-api".
2. However, the service that was running was "starbucks", so when I sent requests to Kong, it did not recognize the routes.


<img width="1512" alt="image" src="https://user-images.githubusercontent.com/49921782/236390929-aa719b2e-73b4-49c3-8d3a-9eb27e193777.png">

Next goal is to start porting SpringMVC.

### Day 3:

1. Ported from client -> cashier
<img width="577" alt="image" src="https://user-images.githubusercontent.com/49921782/236747392-a2144da1-7a74-4d21-897e-7352fc5d3222.png">

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/49921782/236749423-ac075bfe-a4d7-4cc7-b58a-6c2c1b58e95b.png">

### Day 4:

- Continuing porting today and tomorrow
-       Enabling custom orders today

1. Adjusting view
_HTML FORM_
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/49921782/236989429-8e2220b3-85c8-43c0-ad9d-f30e986aed85.png">
2. Adjusting model 
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/49921782/236989744-6dce601c-a5be-47ce-9250-473d47e77cd8.png">


### Day 5

Gonna try to finish porting today.

- For some reason, I am only able to define the internal port for cashier, and NOT the external one for some reason, otherwise I get an error page.

1. Enabling custom order in view
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/0c427992-6d6b-47ed-9298-431bd8e806f8">

2. Ran into an INVALID order request when trying to place an order.
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/a2982357-2e89-49f8-b361-2f403d1ce3c6">

3. API call to place order works (No card ID)
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/6e375daa-884d-4d83-a6de-04672e60adb0">

Accomplished:
- Place order/Delete API order (Remember to update the correct docker images!)

Must finish: 
- Place order duplication
- Get current register order

### Day 6

Unfortunately, I did not have much time to do much, and I am still trying to restore the basic functionality that once existed in the old spring cashier.

1. Clear order works
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/515d1dd5-eae9-4db8-84a8-514e26cb6f81">

2. Fixed double place order. Nothing happens. In the code, if a double place order happens, we instead just make a get call to the order because it already exists.
3. <img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/82ee98c9-40e8-4357-b2ac-79d8e51e1c9e">

3. Get current register works
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/bf1bbb57-96b5-4495-a9f3-a209ac521e6c">

5. We are able to track invalid sizes (Some drinks don't take certain sizes)
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/67773867-68ae-43e1-8b37-3728e3b66d67">

Must finish: 
- Researching Rabbit MQ
- Fix unhandled exceptions: Invalid Size

### Day 7
 
Fixed a bunch of things
- RabbitMQ
- Sender defined in API
- Receiver/Worked defined in own application
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/37cf1e62-b65f-4db0-8f2c-cb2913138b95">

Starbucks Worker
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/025b5850-0d59-4b56-824c-9e697993d96b">
Paid status reflected in cashier
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/57324ab5-6fcb-4c21-81fc-668079b87160">

### Day 8

- Sender is good. Uploads drink order with status "Order being processed" to DB, and sends message to queue.
- Receiver is good. Reads queue, consuems message, and fulfills order. Updates drink order status to "FULFILLED" in DB when worker is done.
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/96f53410-611d-42dc-bf05-a178526f2227">
- Drink order DB works
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/0d9d9515-eef3-4b2d-b95b-3e8c3fa913aa">
- Worker fulfilling order
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/f432142c-c60b-4136-ae77-ab1e475e28ea">
- View status change in MySQL
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/0f87f0b1-2a55-4d67-b7af-be107cb0e529">

TO-DO:
- Remove Hashmap
- User registration
- GKE Deployment

### Day 9 

- Removed hashmap implementation
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/288f3039-c3e7-4a36-83e4-0eba3268d507">

### Day 10

User registration/login/logout
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/b259f020-6537-4666-9f5b-bd6d3a88ab6e">
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/8cc67c21-da5d-4594-9a6b-2c6c19ef6d53">

Active Orders DB to replace Hashamp (previous implementation did not work)
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/63bb04e5-d682-4f5a-a0d3-cf6e694aba27">
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/91869daa-ac3a-4d0d-a20a-6fe3f7e2d4f9">


### Day 11

Overall, I'm finishing up and tweaking the little things that are needed for final deployment such as..
- Port mapping
- Load balancing cashier
- About page support
- Spring sessions
- Non-hard coded credentials


Customized container ports for cashier load balancer and cashier. The reason for this was to avoid conflicting port usage:
Ports to configure: 
- LB: SERVERS_PORT, FRONTEND_PORT
- SPRING CASHIER APP: SERVER_PORT
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/fefaac0c-8034-456e-83f4-c043534dea66">
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/c011bf55-b343-4e66-988f-4abfccb013a3">

Testing workflow using LB'd cashier
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/9606a074-f13d-4eb1-8fa6-869b951501aa">

About page to support seamless login/logout. Had a problem where the user couldn't be directly routed to the main cashier app due to login because no model was being passed, and the best way to get around this was to just route the user to a page that didn't need anything from the model, i.e: the about page.
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/6bb8e766-8800-4679-90de-b0da7e58ac9b">

Spring sessions using JDBC (App should work due to active orders DB implementation)
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/40d8abf6-54ca-49f4-8b7e-8bd46fdfe48b">
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/13dd7541-9018-42bb-b3c9-f12e5bb4551e">
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/8379556b-bd84-4936-b47e-41494a306157">

Changed credentials for MySQL and RabbitMQ respectively. User/pass is student/sjsu respectively.
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/034e85d4-23ac-4150-be26-1e341f1c408c">
<img width="1512" alt="image" src="https://github.com/nguyensjsu/cmpe172-reddesignsguy/assets/49921782/77cd5377-fe73-42e9-accc-03e4cbbb9abf">
