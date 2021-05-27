# ELA-Microservices
This repository explain how to deploy and use ELA Microservices. In this repository, you will find a global description of all public ressources provided by ELA Innovation, how to use it and find usefull links to guarantee success in integrating our products.

- [Introduction](#introduction)
- [Microservices](#microservices)
  - [Architecture](#architecture)
  - [Bluetooth](#bluetooth)
  - [Wirepas](#wirepas)
  - [Authentication](#authentication)
- [Docker](#docker)
  - [Raspbian](#raspbian)
- [Docker-Compose](#docker-compose)
  - [Raspbian (install docker-compose)](#raspbian-install-docker-compose)
- [Deployment](#deployement)
  - [Unix](#unix)

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

This service needs a broker MQTT deployed where data are provided by Wirepas Gateway which publish on it. You can directly have your own instance deployed locally or not. You can find some sample on our github to deploy one on a raspberry pi by following [this link](https://github.com/elaInnovation/mqttbroker-rpi-install).

![here_schematics](https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Wirepas_Microservice_archi_2021.png)

You can find all the documentation, datasheet, functionnalities available from our **Blue Mesh Devices** directly on our [website][here_ela_website].

### Authentication
The **Authentication Module** has no public interface and you won't connect directly to the module. However is presence allow you to use the API from the other module (Bluetooth, Wirepas ...). Before calling any function, you have to call the function Connect from an API (provided for each module) to allow your application to use the entire API. The shematics below show different call to the bluetooth module. 
- Calling the function **StartBluetoothListening** without calling **Connect** will return you an error code ACCESS DENIED
- Before Calling any function you have call **Connect** function with your identifiant to authenticate and use each function from the API 

![here_schematics](https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Authentication_Work_01.png)

## Docker
We use docker to package our application or microservices and its dependencies in a virtual container that can run on Linux computer (more compatibilities will be provided soon). We don't provide here a full docker tutorial, for more details or if you are not familiar with this tool, please refer to the docker [documentation here][here_docker_documentation].

### Raspbian
We prepared a raspberry with a raspbian console image that you can find [here][here_raspbian] (raspios buster i386). Before deploying the container, you need to install all the tools requiered to use **docker** and **docker-compose** on your raspberry.  To do the job, you can follow the procedure just below. 

***Info : We install using the convenience script***
```bash
 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh
```

Note : after installing docker, you can encouter some permissions problems if you run **docker ps** (without sudo). We recommend to fix the issue by adding the current user to the docker group: Run this command in your favourite shell and then completely log out of your account and log back in (or exit your SSH session and reconnect, if in doubt, reboot the computer you are trying to run docker on!):
```bash
sudo usermod -a -G docker $USER
```

***source from docker website : [here](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script)***

On our side, we test it on the following raspberry:
- Raspberry Pi A
- Raspberry Pi 3B+
- Raspberry Pi 4 

## Docker-Compose
Docker Compose relies on Docker Engine for any meaningful work, so make sure you have Docker Engine installed either locally or remote, depending on your setup.

### Raspbian (install docker-compose)
To install docker-compose, you can use directly pip install. If you don't have python3 installed already, you can run the following commands:
```bash
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip
```

Once you have python and pip installed just run the following command:
```bash
sudo pip3 install docker-compose
```
***source from docker website [here]:[https://docs.docker.com/compose/install/]***

## Deployment

### Unix
Now all components are installed, we can use **docker-compose** to deply it on your computer. To proceed, you just need to execute the following command to deploy the requiered containers.
```bash
  docker-compose up -d
```

[here_ela_website]: https://elainnovation.com

[here_ela_github]: https://github.com/elaInnovation

[here_ela_docker]: https://hub.docker.com/u/elainnovation

[here_docker_documentation]:https://docs.docker.com

[here_grpc]: https://grpc.io

[here_raspbian]: https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit
