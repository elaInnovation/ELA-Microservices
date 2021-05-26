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
- **ELA Authentication Microservice** : This is the service dedicated to the authentication.
- **ELA Bluetooth Base Module**: This service is dedicated to the bluetooth device management for all ELA Blue Devices
- **ELA Wirepas Module**: This servic eis dedicated to the wirepas decice management for all ELA Blue Mesh Devices  

### Bluetooth

### Wirepas

## Docker
TODO

## Deployment
TODO

[here_ela_website]: https://elainnovation.com

[here_ela_github]: https://github.com/elaInnovation

[here_ela_docker]: https://hub.docker.com/u/elainnovation

[here_grpc]: https://grpc.io
