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

(Description)

- [Database Model Handler Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-database-model-handler)

(Description)


- [Booking Validator Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-booking-validator)

(Description)

- [Schedule Handler Repository](https://git.chalmers.se/courses/dit355/dit356-2022/t-11/t11-schedule-handler)

(Description)

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

* Sequence Diagram

### Architectural styles
In order to create our project, the combination of following styles has been used:
* **Client/Server:** used to create separation between backend and frontend.
* **Publish/subscribe:** used in order for the components to communicate with each other by publishing and/or subscribing to different topics through a broker, in our case Eclipse Mosquitto.
* **Pipe-and-filter:** used in order to filter data that has be send from one component to another according to certain filter criteria.

## Technical Specifications

### Fault tolerance
(!!! Add here)

### Quality of service
Every component of the system uses QOS level 1 since we want to guarantee that the message is delivered at least once.

### Load Balancer
(!!! Add here)

## Project methodology
(!!! Add here)

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
* [ MongoDB ](https://www.mongodb.com/try/download/community?jmp=nav)
* [ Express.js ](https://expressjs.com/)
* [ Min-priority queue library](https://www.npmjs.com/package/@datastructures-js/priority-queue)

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
				