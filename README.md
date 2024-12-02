RealEstatePropertyManagementCRM

Note: To download the project, click on the ZIP file link. Then click on "View Raw," and the ZIP file will automatically be downloaded.

After downloading the ZIP file, extract it and follow the steps below to get the project running:

Setup Instructions

Ensure you have JDK 11 or later installed on your machine.

Install Maven for dependency management.

Clone the project repository or download the project files.

Import the project into your IDE (e.g., Eclipse or Spring Tool Suite).

Configure your database connection in the application.properties file. For example:

spring.datasource.url=jdbc:mysql://localhost:3306/RealEstatePropertyManagementCRM?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true
spring.datasource.username=<your_username>
spring.datasource.password=<your_password>

Use Maven to install dependencies by running the following command in the terminal:

mvn clean install

Start the application by running the main class (e.g., PropertyManagementApplication.java).

Entity Relationship Diagram

Below is the ER diagram for the project structure:

![image](https://github.com/user-attachments/assets/5f287bfc-e69a-44c8-827b-3fd220915d09)


Database Schema

If ORM does not work, manually create the database schema using the following SQL scripts:

CREATE TABLE Users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    role ENUM('ADMIN', 'MANAGER', 'TENANT') NOT NULL
);

CREATE TABLE Properties (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    location VARCHAR(200) NOT NULL,
    availability BOOLEAN NOT NULL,
    status ENUM('AVAILABLE', 'OCCUPIED', 'MAINTENANCE') NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    manager_id INT NOT NULL,
    FOREIGN KEY (manager_id) REFERENCES Users(id)
);

CREATE TABLE Payments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    paymentDate DATE NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    paymentMethod VARCHAR(50) NOT NULL,
    tenant_id INT NOT NULL,
    property_id INT NOT NULL,
    FOREIGN KEY (tenant_id) REFERENCES Users(id),
    FOREIGN KEY (property_id) REFERENCES Properties(id)
);

CREATE TABLE MaintenanceRequests (
    id INT AUTO_INCREMENT PRIMARY KEY,
    issueDescription VARCHAR(255) NOT NULL,
    status ENUM('PENDING', 'IN_PROGRESS', 'RESOLVED') NOT NULL,
    tenant_id INT NOT NULL,
    property_id INT NOT NULL,
    reportedDate DATE NOT NULL,
    resolvedDate DATE,
    FOREIGN KEY (tenant_id) REFERENCES Users(id),
    FOREIGN KEY (property_id) REFERENCES Properties(id)
);

Running Instructions

Start the application in your IDE or by running:

mvn spring-boot:run

Ensure your MySQL database is running.

Use Postman or any API client to test the endpoints.

Example base URL: http://localhost:4040

API Documentation

AuthController

Base URL: /auth

POST /register

Description: Register a new user.

Request Body:

{
    "email": "xyz@example.com",
    "password": "xyz123"
}

Response: "User registered successfully"

POST /login

Description: Authenticate user and return a JWT token.

Request Body:

{
    "email": "abcd@example.com",
    "password": "abcd123"
}

Response: JWT token as a string.

MaintenanceRequestController

Base URL: /api/maintenance

POST /

Description: Create a new maintenance request.

Request Body:

{
    "issueDescription": "door look in flat 212"
}

Response: Maintenance request object.

GET /

Description: Retrieve all maintenance requests.

Response: List of maintenance requests.

GET /{id}

Description: Retrieve a maintenance request by ID.

URL Example: /api/maintenance/1

Response: Maintenance request object.

PUT /{id}

Description: Update a maintenance request.

URL Example: /api/maintenance/1

Request Body:

{
    "issueDescription": "door looked resolved",
    "status": "RESOLVED",
    "resolvedDate": "2024-12-02"
}

Response: Updated maintenance request.

DELETE /{id}

Description: Delete a maintenance request.

URL Example: /api/maintenance/1

Response: 100 No Content.

PropertiesController

Base URL: /properties

POST /

Description: Add a new property.

Request Body:

{
    "propertyName": "kalyni house",
    "location": "Mumbai",
    "managerId": 01
}

Response: Property object.

PUT /{id}

Description: Update a property.

URL Example: /properties/1

Request Body:

{
    "propertyName": "HDFC Colony Updated",
    "location": "Pune"
}

Response: Updated property object.

DELETE /{id}

Description: Delete a property.

URL Example: /properties/1

Response: 204 No Content.

GET /{id}

Description: Get property by ID.

URL Example: /properties/1

Response: Property object.

GET /

Description: Get all properties.

Response: List of properties.

GET /manager/{managerId}

Description: Get properties managed by a specific manager.

URL Example: /properties/manager/1

Response: List of properties.

Sample Test Data

Register User

{
    "email": "manager@example.com",
    "password": "manager123"
}

Create Maintenance Request

{
    "issueDescription": "Water leakage in kitchen"
}

Add Property

{
    "propertyName": "HDFC Colony",
    "location": "Pune",
    "managerId": 101
}

