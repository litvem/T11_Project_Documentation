# Team-11 Dentistimo Documentation

## Purpose
It is important to go to the dentist for annual checkups and take good care of your teeth.
However, it's not always easy for newcomers in Sweden to get a dentist appointment as most clinics are already full and will reject you 
as a new care-taker. Consequently, they have to manually search online for available time-slots or get a hold of a clinic through 
tiring phone calls, with low success rates.
 
Dentistimo is a web application that provides a dental appointment booking service for residents of Gothenburg. The application offers the option to check basic information for the clinics on the map and book an appointment by choosing a time and filling in personal details. 

## Components

Below links represent the project structure and will guide you to designated repositories. In each repository, a detailed description of respected component responsibilities are provided.

- [Web Application Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-web-application)

The Web Application is the user-interface where available time slots for appointments can be spotted and bookings can be made. 
It consists of a vue frontend and an express backend.

- [Database Model Handler Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-database-model-handler)

The Database Model Handler defines the schema for the clinics and bookings, which is used to persist clinic information and bookings in the MongoDB database.


- [Booking Validator Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-booking-validator)

The Booking Validator queues booking requests from the web application and sends them to the DB Model Handler to persist the bookings in the database. 
It also sends a confirmation email for every successful booking.

- [Schedule Handler Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-schedule-handler)

The Schedule Handler stores clinic information and bookings from the MongoDB database to generate schedules for user-desired time intervals. 
The generated schedule is passed on to the frontend.


## Software requirement specification (SRS)

### Functional requirements
1. The system must allow users to book dentist appointments.
2. The system must have a map-view over Gothenburg that can be navigated.
3. The system must be able to find available times on a user-specified date.
4. The system must provide a confirmation/rejection for book appointments.
5. The system database must contain all dentist related information.
6. The system must track the availability of free time-slots for multiple dental offices.
7. The system must provide a graphical representation of the available time-slot for all the registered dental offices. 
8. The system must react to simultaneous booking by changes in the availability visible to the user.

### Non-functional requirements
1. The system should be easy for users to use.
2. The system should be fault tolerant.
3. Users should be able to book an appointment in less than 7 clicks.
4. Architectural styles such as Pipe-and-Filter, Publish/Subscribe and Client/Server are supposed to be combined.

### Constraints
#### a) Architectural Constraints
1. The system must be composed of at least 4 system components.
2. At least 4 components must be distributed.
3. There must be a middleware based on the MQTT protocol
4. All components shall be able to handle errors *“such as wrongly formatted data inputs or out of bounds inputs for the defined interfaces”*.

#### b) Domain Constraints
1. Each appointment is 30 minutes long.
2. Appointment can start every half or full hour.
3. Each dentist has a one hour lunch break.
4. Each dentist has a 30 minutes fika break.
5. Each clinic has a name, an owner, an address and coordinates, a city (Gothenburg), opening hours and number of dentist.

#### c) Request Format Constraints
1. Booking request shall follow the following format:
```
{
  "userid": 12345,
  "requestid": 13,
  "dentistid": 1,
  "issuance": 1602406766314,
  "date": "2020-12-14"
}
```
2. The system response in case of a successful booking shall follow the following format:
```
{
  "userid": 12345,
  "requestid": 13,
  "time": "9:30" # alt "none" if failed
}
```

## Software Architecture

### Architectural drivers
(!!!To add)

### Architectural Significant Requirements (ASR)
* Performance: we need to be able to handle different loads of request.
* The use of Mqtt and the Publish/Subscribe architecture is essential to our architecture design.
* Fault tolerance: *“The handling of resources shall be performed in a mindful way (e.g., components who are no longer active should unsubscribe from the MQTT broker)”*. 

### Architectural Diagrams
* Component Diagram

![Component Diagram](./assets/Component_Diagram.png)

* Deployment Diagram

![Deployment Diagram](./assets/deployment_diagram.png)

* Sequence Diagram

### Architectural styles
In order to create our project, the combination of following styles has been used:
* **Client/Server:** used to create separation between backend and frontend.
* **Publish/subscribe:** used in order for the components to communicate with each other by publishing and/or subscribing to different topics through a broker, in our case Eclipse Mosquitto.
* **Pipe-and-filter:** used in order to filter data that has be send from one component to another according to certain filter criteria.

## Technical Specifications

### Fault tolerance and Load Balancer
The system uses the **Circuit Breaker** pattern to implement fault
tolerance and the chosen component for this pattern is 
the [ Booking validator ](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-booking-validator).
This component also implements a minimum heap priority queue ordered by issuance which works as a load balancer,
avoiding overloading the component while ensuring that non-unintentional duplicates are stored by having a synchronous 
communication with the DB Model Handler. 

The booking validator combines the circuit breaker with the load balancer in order to track the load of the component.
When there is a high load in the components and the load balancer is at maximum capacity the circuit breaker
enters an open state during 30 seconds. After the timeout the circuit breaker enter a half-open state which has been modified.
Instead of verifying that the next request is successful the half-open state checks if the min heap priority queue 
is at specified threshold capacity level. If the queue size is above the specified threshold capacity the breaker enter 
the open state again and if the queue size is below the specified threshold capacity the breaker enter to a close state 
instead.

More information regarding the circuit breaker pattern is 
found [ here ](https://martinfowler.com/bliki/CircuitBreaker.html).

### Quality of service
The messages related to the booking use a QoS level 2 providing a warranty that the message is delivered only once. QoS 2 level has a higher overhead and takes more time to complete but it ensures that no double booking is made while reducing the load on the Booking validator component.  
Messages that are not related to the booking storing process use a QoS level 1 assuring that the message is delivered at least once. 

## Project methodology

We had an agile approach and adapted Scrum practices to our development process. Scrum roles were assigned to team members and remained fixed during development.
We delivered the system incrementally in several iterations. Overall, we had four sprints where each sprint had a duration of two weeks. 
At the beginning of each sprint, we did our sprint planning and used our Trello board for managing the tasks. 
Weekly Scrum meetings were held twice a week to report progress of each team member and possible obstacles they might have faced. 
At the end of each sprint, we had our Sprint Retrospective with the product owner.


## Software tools used

### Project Management Tools
* Project board: [Trello](https://trello.com/b/FKnvU7Dd/t-11)
* Diagram tool: [ draw.io ](https://app.diagrams.net/?src=about)
* Document sharing: [ Google Drive ](https://drive.google.com/drive)
* Communication tools: [ Discord ](https://discord.com/), [ Slack ](https://slack.com/)

### Product Development Tools
* [ NPM ](https://www.npmjs.com/)
* [ Vue.js](https://vuejs.org/)
* [ Vite ](https://vitejs.dev/)
* [ Bootstrap ](https://getbootstrap.com/)
* [ BootstrapVue ](https://bootstrap-vue.org/)
* [ VueGeolocation API ](https://developers.google.com/maps/documentation/javascript/cloud-setup)
* [ VueGoogleMaps ](https://developers.google.com/maps/documentation/javascript/cloud-setup)
* [ Eclipse Mosquitto ](https://mosquitto.org/)
* [ MQTT.js ](https://www.npmjs.com/package/mqtt)
* [ Opossum ](https://nodeshift.dev/opossum/)
* [ Nodemailer ](https://nodemailer.com/about/)
* [ Mongoose ](https://mongoosejs.com/)
* [ MongoDB Atlas ](https://www.mongodb.com/atlas/database)
* [ Express.js ](https://expressjs.com/)
* [ Min-priority queue library](https://www.npmjs.com/package/@datastructures-js/priority-queue)

### MongoDB Atlas Setup
Our project requires the user to set up a MongoDB Atlas account and store the [dentists](https://raw.githubusercontent.com/feldob/dit355_2020/master/dentists.json) 
and bookings for the system to work in a local PC. Follow the steps below to create a free Atlas account and connect our system to the cloud database:

1. Navigate to [MongoDB Atlas website](https://www.mongodb.com/cloud/atlas/register).
2. Preferably sign up with an existing Google account or create an account manually by filling in your details.
3. You will be forwarded to the "Create a Database" view. Otherwise, navigate to Database > Build a Database.
4. Choose to create a free and shared MongoDB Atlas database.
5. Choose the cloud provider aws and the region Stockholm (eu-north-1). Keep all other default settings (e.g., M0 Sandbox free tier, cluster name Cluster0) and click Create Cluster (takes a few minutes).
6. Create a new Database user by entering Username and Password (avoid special characters for mongoose compatibility) and click "Create User" button.
7. Choose "My Local Environment" to connect from.
8. Enter ```0.0.0.0/0``` for the IP Address and click Add IP Address button.
9. Navigate to Database and click the "Connect" button from the created database, then choose "connect your application" on the  pop-up.
10. Copy and store the auto-generated connection string.

#### Insert Dentist Information in the database
1. Navigate to the Database and click the "Browse Collections" button, then click "Add My Own Data" button
2. Set the database name to "tests" and the collection name to "dentists"
3. Go to [dentists.json](https://raw.githubusercontent.com/feldob/dit355_2020/master/dentists.json) and copy the dentist data inside the list including the square brackets.
4. Go back to the website and click on "INSERT Document" from the "dentists" collection, then paste the copied list and click "Insert" button.

#### Connect Dentistimo system to MongoDB Atlas
1. Open [Schedule Handler](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-schedule-handler) and [Database Model Handler](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-database-model-handler) in IDE, or clone the repositories if you don't have it locally.
2. Add a ```".env"``` file in the root folder of the components containing the following key:
```dotenv
MONGO_ATLAS_URI="<Insert copied string from step 10>"
```
3. Replace the ```"<password>"``` placeholder with your personal password from step 6.

## Team Members

| Names                                 | Roles         |
|---------------------------------------|---------------|
| Saif Sayed                            | Developer     |
| Robert Einer                          | Developer     |
| Emma Litvin                           | Scrum Master  |
| Danila Baryshev                       | Developer     |
| Bao Quan Lindgren                     | Developer     |
| Nicole Andrea Quinstedt               | Developer     |
| Luiz Eduardo Philippi Rosane          | Product Owner |
| Khaled Adel Saleh Mohammed Al-Baadani | Developer     |
