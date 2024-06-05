# Overview

The site hosts all important documents required to design and implement 
the smart map system.  It is powered by [Writerside](https://www.jetbrains.com/help/writerside/getting-started.html), and is deployed through
GitHub action. 

To see how the system work, check the ["Software Requirement Specification"](Software-Requirement-Specification.md)


## Architecture

The system includes 2 logic part, client and server.
The server should only provide general information since it doesn't and 
cannot know how the client work, for instance, we use LEDs to show 
demonstrated the trains locations.

<warning>a structured data is need. We need to design it before going to implement phase.</warning>

### Client

Subscribe the server's data and show the train's location.

### Server

Publish the train's real-time data. 


## Technology

1. MQTT(Eclipse Mosquitto)