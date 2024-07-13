# Software Requirement Specification


## Introduction

### Purpose

The purpose of this document is to provide a detailed specification for the Smart Map System, which tracks
and displays real-time train locations. This document is intended for use by developers, testers, and stakeholders
involved in the development and maintenance of the system.

### Scope

The Smart Map System consists of two main components: a client application that displays train locations and a server
that processes and publishes real-time train data. The system aims to provide accurate and timely information to users, 
improving their experience. 

### Definitions, Acronyms, and Abbreviations

* **GTFS**: General Transit Feed Specification.
* **MQTT**: Message Queuing Telemetry Transport.
* **SQLite3**: A C library that provides a lightweight, disk-based database.
* **API**: Application Programming Interface.
* **LED**: Light Emitting Diode.
* **DEV**: Development mode.
* **PROD**: Production mode.

### References

1. [Writer Side Documentation](https://www.jetbrains.com/help/writerside/getting-started.html)
2. [mosquitto](https://mosquitto.org/documentation/)
3. [Sqlite Index](https://www.sqlite.org/lang_createindex.html)
4. [Google GTFS](https://developers.google.com/transit/gtfs/reference)
5. [NeoPixel](https://learn.adafruit.com/neopixels-on-raspberry-pi/python-usage)

## Overall Description

### Product Perspective

The Smart Map System is an independent application that integrates real-time train with a client application for 
visualization. It is designed to be extensible and scalable to handle the large GTFS data. 

### Product Functions

* **Client**:
  * Subscribe to server data.
  * Display train locations using LEDs.
* Server:
  * Establish connection with MQTT broker.
  * Query the database for real-time schedule train data.
  * Modify schedule data using real-time data from Google.
  * Publish train data in Json format to the broker.


### Operating Environment

* **Client**: Runs on RaspberryPi capable of connecting to the MQTT broker and displaying data.
* **Server**: Runs on RaspberryPi or another computer that can access the MQTT broker. It can be in the same device with
            the client. 
* **Broker**: is an instance of mosquitto, can be accessed by both client and server.

### Design and Implementation

* The server must handle real-time data processing efficiently.
* The client application must be responsive and display real-time updates after parsing the data from broker.
* The database is stored on Google Drive and accessed via `gdown`.


### Assumption and Dependencies

* The MQTT broker (Eclipse Mosquitto) is assumed to be reliable and available.
* Real-time data from Google is assumed to be accurate and accessible.
* The Client can access a privilege permission to be able to control the leds.

## System Features

### Client Features

#### Subscribe to Server Data

* **Description**: The client subscribes to the server to receive real-time train data.
* **Inputs**: MQTT broker address, connection parameters.
* **Outputs**: Real-time train data.
* **Dependencies**: MQTT connection.

#### Display train location using LEDs

* **Description**: The client displays train locations using LEDs.
* **Inputs**: Real-time train data from the server.
* **Outputs**: LED signals indicating train locations.
* **Dependencies**: LED hardware, real-time data, sudo permission.


### Server Features


#### Establish connection with MQTT broker 

* **Description**: The server connects to the MQTT broker.
* **Inputs**: Broker Address, connection parameters.
* **Outputs**: Connection status.
* **Dependencies**: MQTT broker availability.

#### Query the database for real-time schedule train data

* **Description**: The server queries the database to retrieve real-time train data.
* **Inputs**: SQL queries, database connection.
* **Outputs**: Real-time train data.
* **Dependencies**: Database accessibility, GTFS data.

#### Modify schedule data using real-time data from Google

* **Description**: The server uses real-time data from Google to update the train schedule data.
* **Inputs**: Real-time data from Google API.
* **Outputs**: Updated train schedule data.
* **Dependencies**: Access to Google API, real-time data accuracy, no more than 300ms.

#### Publish train data in Json format to the broker

* **Description**: The server publishes train data in JSON format to the MQTT broker.
* **Inputs**: Real-time train data.
* **Outputs**: JSON messages.
* **Dependencies**: MQTT connection.

## External Interface Requirements

### User Interfaces
* **Client Application Interface**: Display train locations, user-friendly design.

### Hardware Interfaces
* LED Hardware: Used by the client to display train locations.
* Client Host: Hosts the Client. The host has port to control the LEDs.

### Software Interfaces
* **MQTT Broker (Eclipse Mosquitto)**: For message communication between server and client.
* **SQLite3 Database**: For storing and querying train data.
* **Google Drive**: For storing the SQLite3 database.
* **Google API**: For accessing real-time data to update train schedules.


### Communication Interfaces

* **Internet Connection**: Required for server and client communication, database access, and real-time data retrieval
    from Google.

## Other Nonfunctional Requirements


### Performance Requirements
* The system should query and publish data within **_600ms_**.
* The client should display real-time updates with minimal latency.
### Security Requirements
* Data transmission between server and client must be secure.
* Access to the database should be restricted to authorized personnel.
* Access to the Google API should be secure and authenticated.
### Software Quality Attributes
* **Reliability**: The system should provide accurate and consistent data.
* **Scalability**: The system should handle an increasing number of users and larger datasets.
* **Maintainability**: The system should be easy to maintain and update.