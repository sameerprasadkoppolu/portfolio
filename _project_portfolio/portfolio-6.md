---
title: "Restaurant Reservation Management System"
excerpt: "A comprehensive dine-in and reservation management system for restaurants that provides a structured approach to data storage and retrieval for all entities involved in the restaurant business."
collection: portfolio
---


## GitHub Repository:
[Repository](https://github.com/sameerprasadkoppolu/Restaurant-Reservation-Management-System)  


## Introduction:
This project aims to create a comprehensive restaurant dine-in and reservation management system to offer users a seamless experience when interacting with various restaurant-related services. It provides a structured approach to data storage and retrieval for all entities involved in the restaurant business. At a high level, the system allows customers to place dine-in orders and make restaurant reservations, while the restaurants can also track customer orders and reservations.


## Conceptual Database Design:
The following Entity Relationship Diagram (ERD) implemented using UML notation showcases the design of the database highlighting the entities and the relationships between them.  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/2838e69f-a3b6-41dd-8c2e-b6cb56ed59aa)  


## User Flow:
* When the application is started and once the connection to the DB is achieved, the user will be prompted for a username. At this point, if a first-time user is interacting with the application, no username will be found and the user will have to create a customer account. If the user is not a first-time user, then he/she will be shown directly to the application menu.
* The Application Menu consists of 26 options that a user can pick. These encapsulate the CRUD operations that a user can perform. After every option is chosen and executed, the user will be prompted again to make a choice or exit the application
* A user can search for restaurants, view the menus of each restaurant, the respective cuisines of each restaurant, the menu items in each menu of each restaurant, and the ingredients in each menu item.
* The User can add payment information, update payment information, and delete payment information. The payment information includes the credit card details of the user.
* The User can also place orders for menu items from specific restaurants using their payment information
* The User can also create, view, update, and delete reviews for a restaurant. A user can only create a review for a restaurant if he/she has placed at least 1 order at that restaurant.
* The user can also create reservations, update and delete any upcoming reservations, and also view upcoming and past reservations.
* For each of the 26 options, the user will have to type in the information that they are being prompted for.
* A user also has the option to delete their customer account, and if they decide to do so, their account will be deleted and they will be exited from the application. The next time they open the application, they will be prompted for a username, and since they have no valid username, they will be asked to create one. Thereafter, the application menu with the 26 options will be displayed.

The following diagram highlights the user flow in the application.  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/fa097a87-4809-4d6f-bbca-7d9e7406422e)


## Using the Database:
* The Database was built using MySQL. Using MySQL workbench, one can import the Database Schema and Dump with some sample data in the database. Dump is available in the aformentioned linked repository.
* CRUD operations are executed using a Python Application with 'pymysql' being used to connect to the database with appropriate login credentials. The application can be run and interacted with using a Command Line Interface.
* Once connection to DB is established, you will again be prompted for a username - this is the username for a customer account. Since a first-time user of the application will not have a customer account, you will be prompted to create a customer account with a unique username.
* CRUD operations are enabled using functions and procedures that are provided in the SQL database dump.
