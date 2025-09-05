Helpdesk Ticket Tracker

This project was developed as part of a DBMS mini project using Java and Oracle Database.
It is designed to manage helpdesk tickets raised by users and handled by administrators or agents.
The system provides functionality to create, assign, update, and track tickets, as well as add comments for better communication.

Database Tables
USERS
User_id (Primary Key)
Name
Role (Customer, Agent)
Email
Created_at

DEPARTMENTS
Dept_id (Primary Key)
Name
User_id (Foreign Key, department manager)

TICKETS
Ticket_id (Primary Key)
User_id (Foreign Key, ticket creator)
Dept_id (Foreign Key, department handling ticket)
Subject
Description
Status (Open, In Progress, Closed)
Priority (Low, Medium, High)
Created_at
Updated_at

TICKET_COMMENTS
Comment_id (Primary Key)
Ticket_id (Foreign Key)
User_id (Foreign Key)
Message

TICKET_ASSIGNMENTS
Assignment_id (Primary Key)
Ticket_id (Foreign Key)
Assigned_at

Relationships
A user can raise multiple tickets.
Each ticket belongs to a department.
A ticket can have multiple comments.
Tickets are assigned to agents through ticket assignments.
Each department can be managed by a user.

Features
User registration and login
Create and manage tickets
Assign tickets to agents
Add and view comments on tickets
Track ticket status and history

Technology Used
Java (JDBC)
Oracle Database
Eclipse
Git and GitHub

Setup Instructions
Clone this repository into your system.
Open the project in a Java IDE (Eclipse).
Set up the Oracle database and run the SQL scripts for tables.
Update the database connection details in the Java code.
Compile and run the project.
Here below are the tables to create and the values to be inserted.
-- USERS table
CREATE TABLE Users (
    user_id      NUMBER PRIMARY KEY,
    name         VARCHAR2(100),
    role         VARCHAR2(50),
    email        VARCHAR2(100) UNIQUE,
    created_at   DATE DEFAULT SYSDATE
);

-- DEPARTMENTS table
CREATE TABLE Departments (
    dept_id      NUMBER PRIMARY KEY,
    name         VARCHAR2(100),
    user_id      NUMBER,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

-- TICKETS table
CREATE TABLE Tickets (
    ticket_id    NUMBER PRIMARY KEY,
    user_id      NUMBER,
    dept_id      NUMBER,
    subject      VARCHAR2(200),
    description  VARCHAR2(1000),
    status       VARCHAR2(50),
    priority     VARCHAR2(20),
    created_at   DATE DEFAULT SYSDATE,
    updated_at   DATE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (dept_id) REFERENCES Departments(dept_id)
);

-- TICKET_COMMENTS table
CREATE TABLE Ticket_Comments (
    comment_id   NUMBER PRIMARY KEY,
    ticket_id    NUMBER,
    user_id      NUMBER,
    message      VARCHAR2(1000),
    FOREIGN KEY (ticket_id) REFERENCES Tickets(ticket_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

-- TICKET_ASSIGNMENTS table
CREATE TABLE Ticket_Assignments (
    assignment_id NUMBER PRIMARY KEY,
    ticket_id     NUMBER,
    assigned_at   DATE DEFAULT SYSDATE,
    FOREIGN KEY (ticket_id) REFERENCES Tickets(ticket_id)
);

-- Insert Users
INSERT INTO Users (user_id, name, role, email) VALUES (1, 'Alice', 'Customer', 'alice@example.com');
INSERT INTO Users (user_id, name, role, email) VALUES (2, 'Bob', 'Agent', 'bob@example.com');
INSERT INTO Users (user_id, name, role, email) VALUES (3, 'Charlie', 'Admin', 'charlie@example.com');

-- Insert Departments
INSERT INTO Departments (dept_id, name, user_id) VALUES (1, 'IT Support', 3);
INSERT INTO Departments (dept_id, name, user_id) VALUES (2, 'HR Services', 3);

-- Insert Tickets
INSERT INTO Tickets (ticket_id, user_id, dept_id, subject, description, status, priority)
VALUES (101, 1, 1, 'Laptop not working', 'My laptop does not start properly.', 'Open', 'High');

INSERT INTO Tickets (ticket_id, user_id, dept_id, subject, description, status, priority)
VALUES (102, 1, 2, 'Leave application issue', 'Unable to submit leave request in portal.', 'Open', 'Medium');

-- Insert Ticket Comments
INSERT INTO Ticket_Comments (comment_id, ticket_id, user_id, message)
VALUES (201, 101, 2, 'Please bring your laptop to the IT desk tomorrow.');

INSERT INTO Ticket_Comments (comment_id, ticket_id, user_id, message)
VALUES (202, 102, 3, 'We are checking the HR portal issue.');

-- Insert Ticket Assignments
INSERT INTO Ticket_Assignments (assignment_id, ticket_id, assigned_at)
VALUES (301, 101, SYSDATE);

INSERT INTO Ticket_Assignments (assignment_id, ticket_id, assigned_at)
VALUES (302, 102, SYSDATE);

