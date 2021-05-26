# ELA-Microservices
This repository explain how to deploy and use ELA Microservices. In this repository, you will find a global description of all public ressources provided by ELA Innovation, how to use it and find usefull links to guarantee success in integrating our products.

- [Introduction](#introduction)
- [Microservices](#microservices)
  - [Architecture](#architecture)
  - [Bluetooth](#bluetooth)
  - [Wirepas](#wirepas)
- [Docker](#docker)
- [Deployment](#deployement)

## Introduction
ELA developped different microservices to create interfaces with the hardware provided. You can find all usefull informations in our products by visiting our website [here][here_ela_website]. But once you get the products, you can find here something helping you to integrate and get the information from our devices. By using our [github][here_ela_github], you can find samples in different langage to understand how you can integrate it on your side. But we provided two some [docker][here_ela_docker] to get our microservices on different technologies to garantee to get the information from our tags. 

If you are a developper and you want to go further in the integration, you are in the right place.

## Microservices
So, we developped at ELA some microservices using .NetCore, C# and gRPC. Maybe you are not familliar with Microsoft technologies, but this is not a problem. Why? because by using gRPC, we implement the server side and all the acquisition loops or connection to our device on our side. And then we provide a gRPC API with all the functionnalities you need to get information or connect. But before to go further, you should have a look on what is [gRPC][here_grpc] if you are not familliar with it.

Now if it's ok, we can talk about our microservices. So, we package everything using docker. You can find them directly on our [dockerhub][here_ela_docker]. However, it will be usefull to use this repository and the  ressources available to install it by using docker compose. You will find all the instruction in the following paragraph, but before let's talk about the different services and the API provided.

### Architecture
The main architecture of the docker we provide is the one describe the schematics below. We deliver the following services:
- **ELA Authentication Microservice** : This is the service dedicated to the authentication and will allow you to connect to Bluetooth or Wirepas microservice.
- **ELA Bluetooth Base Module**: This service is dedicated to the bluetooth device management for all **ELA Blue** Devices
- **ELA Wirepas Module**: This servic eis dedicated to the wirepas decice management for all **ELA Blue Mesh** Devices 

![here_schematics](https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Microservices_Main_archi_2021.png)

All this package is available by using docker compose. But to use the different modules, you need to contat ELA innovation to get a certificate which allow you to use the microservices. This certificate allow you to connect to a service and use the main functionnalities provided by the API. We will talk about the authentication module and the role later in the documentation.

### Bluetooth
The **Bluetooth Base Module** allow you to communicate with the **ELA Blue Devices**. The main functionnalities (which are the API functions) are quite simple:
- Start Bluetooth Scanner: use the bluetooth layer on the machine where the service is intalled to start a bluetooth scan
- Stop Bluetooth Scanner: use the bluetooth layer on the machine where the service is intalled to stop a bluetooth scan
- Get Scanned Devices : Get the main informations about the tag scanned between a start and stop
- Connect to a Specific Tag: use the connection mode implemented in **ELA Blue Device** (Nodic UART Service) to send specific commands and get a response from the tag

You can find all the documentation, datasheet, functionnalities available from our **Blue Devices** directly on our [website][here_ela_website].

### Wirepas
The **Wirepas Base Module** allow you to communicate with the **ELA Blue Mesh Devices**. The main functionnalities (which are the API functions) are quite simple:
- Start Wirepas Dataflow: subscribe to a target MQTT broker where Wirepas data are published to get and interpret the flow. Open a streaming channel on this Dataflow 
- Stop Wirepas Dataflow: cancel the streaming dataflow and disconnec from the MQTT Broker
- Send a command to a Specific Tag: use the MQTT broker to publish command on the Wirepas network. The commands are the one implemented into **ELA Blue Mseh Device**

This service 

![here_schematics](https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Wirepas_Microservice_archi_2021.png)

You can find all the documentation, datasheet, functionnalities available from our **Blue Mesh Devices** directly on our [website][here_ela_website].

## Docker
TODO

## Deployment
TODO

[here_ela_website]: https://elainnovation.com

[here_ela_github]: https://github.com/elaInnovation

[here_ela_docker]: https://hub.docker.com/u/elainnovation

[here_grpc]: https://grpc.io
