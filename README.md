# PHP Hackathon
This document has the purpose of summarizing the main functionalities your application managed to achieve from a technical perspective. Feel free to extend this template to meet your needs and also choose any approach you want for documenting your solution.

## Problem statement
*Congratulations, you have been chosen to handle the new client that has just signed up with us.  You are part of the software engineering team that has to build a solution for the new client’s business.
Now let’s see what this business is about: the client’s idea is to build a health center platform (the building is already there) that allows the booking of sport programmes (pilates, kangoo jumps), from here referred to simply as programmes. The main difference from her competitors is that she wants to make them accessible through other applications that already have a user base, such as maybe Facebook, Strava, Suunto or any custom application that wants to encourage their users to practice sport. This means they need to be able to integrate our client’s product into their own.
The team has decided that the best solution would be a REST API that could be integrated by those other platforms and that the application does not need a dedicated frontend (no html, css, yeeey!). After an initial discussion with the client, you know that the main responsibility of the API is to allow users to register to an existing programme and allow admins to create and delete programmes.
When creating programmes, admins need to provide a time interval (starting date and time and ending date and time), a maximum number of allowed participants (users that have registered to the programme) and a room in which the programme will take place.
Programmes need to be assigned a room within the health center. Each room can facilitate one or more programme types. The list of rooms and programme types can be fixed, with no possibility to add rooms or new types in the system. The api does not need to support CRUD operations on them.
All the programmes in the health center need to fully fit inside the daily schedule. This means that the same room cannot be used at the same time for separate programmes (a.k.a two programmes cannot use the same room at the same time). Also the same user cannot register to more than one programme in the same time interval (if kangoo jumps takes place from 10 to 12, she cannot participate in pilates from 11 to 13) even if the programmes are in different rooms. You also need to make sure that a user does not register to programmes that exceed the number of allowed maximum users.
Authentication is not an issue. It’s not required for users, as they can be registered into the system only with the (valid!) CNP. A list of admins can be hardcoded in the system and each can have a random string token that they would need to send as a request header in order for the application to know that specific request was made by an admin and the api was not abused by a bad actor. (for the purpose of this exercise, we won’t focus on security, but be aware this is a bad solution, do not try in production!)
You have estimated it takes 4 weeks to build this solution. You have 2 days. Good luck!*

## Technical documentation
### Data and Domain model
In this section, please describe the main entities you managed to identify, the relationships between them and how you mapped them in the database.

I managed to identify the following entities:
- Bookings 
- Programmes 
- Rooms 
- Sports 
- Users 
- Roles 

The relationships between entities are the folloing:
- Bookings and Rooms -> many to many 
- Bookings and Users -> many to many 
- Programmes and Bookings -> one to many 
- Programmes and Rooms -> many to many 
- Programmes and Sports -> many to many 
- Roles and Users -> one to many 

Entities mapped in database: 
- Bookings (id int pk auto increment, user_id int not null fk, programmes_id int not null fk)
- Programmes (id int pk auto increment, participants int not null, start_at datetime, end_at datetime, room_id int not null fk, sport_id int not null fk)
- Rooms (id int pk auto increment, room_number)
- Sports (id int pk auto increment, sport_name varchar)
- Users (id int pk auto increment, name varchar not null, cnp varchar not null, role_id default 1 fk)
- Roles (id int pk auto increment, role_name varchar not null)

### Application architecture
In this section, please provide a brief overview of the design of your application and highlight the main components and the interaction between them.

I made this project using Laravel framework and the database using MySql. The main components of this project are the controllers, models, and routes. Basically in controllers I wrote the logic part and the validations. In models I made the relationships between models and I mapped them and in routes I defined the http requests to access and test the application.

###  Implementation
##### Functionalities
For each of the following functionalities, please tick the box if you implemented it and describe its input and output in your application:

[x] Brew coffee \
[x] Create programme - We have to access /api/programmes/1/add-programmes to create a new programme. Here we have to fill the following fields: participants, start_at, end_at, room_id, sport_id. If based on validation everything is right the new programme will be saved in database else the application will show an error message. That "1" inside the url represents the user with admin role.  \
[x] Delete programme - For deleting we have to access api/programmes/1/delete-programmes/40. The "40" number represents the programme id. In case we have bookings registered on the programme we want to delete then we will delete all the bookings that are related to the programme before deleting the programme.\
[x] Book a programme - /api/bookings/create-booking. Here we have to fill cnp and programme_id fields. If the validations are right the booking will be saved else the application will show error messages.

##### Business rules
Please highlight all the validations and mechanisms you identified as necessary in order to avoid inconsistent states and apply the business logic in your application.

Validations:
- check if some requested values exists in database
- CNP to check if it's unique inside database, the length is 13 and it's made out of numbers
- check if the user is an administrator for creating and deleting programmes
- check if the user can be registered at the programme
- check if the programme has related bookings 
- check the booking overlap time validation for every possibility

##### 3rd party libraries (if applicable)
Please give a brief review of the 3rd party libraries you used and how/ why you've integrated them into your project.

##### Environment
Please fill in the following table with the technologies you used in order to work at your application. Feel free to add more rows if you want us to know about anything else you used.
| Name | Choice |
| ------ | ------ |
| Operating system (OS) | Windows 10 |
| Database  | MySQL 8.0 |
| Web server| Apache |
| PHP | 7.4.1 |
| Framework | Laravel 8.0 |
| IDE | Atom |
| API Client | Postman |

### Testing
In this section, please list the steps and/ or tools you've used in order to test the behaviour of your solution.

I used Postman to test the API calls.

## Feedback
In this section, please let us know what is your opinion about this experience and how we can improve it:

1. Have you ever been involved in a similar experience? If so, how was this one different? \
    No, this is my first time in a hackathon.
2. Do you think this type of selection process is suitable for you? \
    Yes, because I learned new things.
3. What's your opinion about the complexity of the requirements? \
    If you are ready you can go through the tasks.
4. What did you enjoy the most? \
    The meetings and the booking overlap validation.
5. What was the most challenging part of this anti hackathon? \
    The bookings overlap validation.
6. Do you think the time limit was suitable for the requirements? \
    Yes.
7. Did you find the resources you were sent on your email useful? \
    Yes.
8. Is there anything you would like to improve to your current implementation? \
    Some validations. I can think of other validation methods.
9. What would you change regarding this anti hackathon? \
   Nothing, it was awesome. Keep going like that!
