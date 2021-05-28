# ELA Wirepas Base Microservice

Here, you will find all the informations relatives to the **Wirepas Base Microservice** developped by ELA Innovation. Once the installation complete, this microservice allow you to use a wirepas network and a MQTT broker to release simple functions like getting information from your Wirepas Network or sending commands to an **ELA Blue Mesh Device**.

Don't forget that this service needs a MQTT broker deployed where data are provided by Wirepas Gateway which publish on it. You can directly have your own instance deployed locally or not. You can find some sample on our github to deploy one on a raspberry pi by following [this link](https://github.com/elaInnovation/mqttbroker-rpi-install).

<p align="center">
  <img width="650" src="https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Wirepas_Microservice_archi_2021.png">
</p>

Remeber, all information to get data from the service are describe here in the description and further in the API description. But if you want to have more information about Wirepas, Wirepas Network or develop your own tools, you can do it by following this link on the [Wirepas Github][here_wirepas_github].

## Service Description
We implement on server side a service which deal with the MQTT broker and unwrap all informations from a Wirepas network according to their protocol description. Using or software you can listen or publish data on Wirepas Network to read **ELA Blue Mesh Device**. The main functionnalities we expose through our API allow a user to:
- Start flow by subscribing to the MQTT topic related to specific sensor data 
- Stop a flow by unsubscribing the MQTT topic 
- Get the latest data for subscribed topics 
- Publish to a tag to perform an operation (e.g. turn on the LED) 
- Recover the state of the micro-service 
- Set filters for MQTT topics (optional on separate function) 
- Filter reset (optional on separate function) 

We keep all this functionnalities as simple as possible. The API as only the following function:
- **StartWirepasDataFlow** : to start listnening the wirepas data flow published on the MWTT broker 
- **StopWirepasDataFlow** : to stop a current instance of listning wirepas flow (engaged with StartWirepasDataFlow)
- **SendElaWirepasCommand** : to send a command through the wirepas network and get the result from the tag
- **GetServiceStatus**: to get the service information (version, os, state ...)
```proto
// The BackendConfig service definition.
service ElaWirepasPublicService {

  // Wirepas flow management
  rpc StartWirepasDataFlow(ElaWirepasDataRequest) returns (stream ElaWirepasDataPacket) {}
  rpc StopWirepasDataFlow(ElaWirepasDataRequest) returns (ElaCommon.ElaError) {}

  // Wirepas Commands
  rpc SendElaWirepasCommand(SendPacketReq) returns (SendPacketResp) {}

  // Wirepas Monitoring
  rpc GetServiceStatus(ElaCommon.ElaInputBaseRequest) returns (ElaWirepasMicroserviceStatus) {}
}
```
You can find all the information about the API and object here.

Through the function available in the API, we can describe the state machine associated to the **Bluetooth Base Microservice**. 
<p align="center">
  <img width="650" src="https://github.com/elaInnovation/ELA-Microservices/blob/master/Documentation/Wirepas%20Base/Images/Wirepas_Service_State_Machine_01.PNG">
</p>

We keep all this functionnalities as simple as possible. The API as only the following function:
- **Pending**: Waiting for a feature request 
- **Subscribed**: The subscription for requested topic is done 
- **Published**: A requested topic has been published 
- **Publishing**: Sent a command to the tag as a publish request, not yet published 

## Authentication
To use the different function from the API, you need use the authentication function provided in the Wirepas Base API. Two function are here to do the authentication job:
- Connect: provide your login and password information and get a session token to use all the functionnalities provided by Bluetooth Base API
- Disconnect: diconnect your client from the bluetooth module and delete the availability of your session token
```proto
// The BackendConfig service definition.
service ElaWirepasPublicService {

  // authentication functions
  rpc Connect(ElaAuthentication.ElaAuthenticationRequest) returns (ElaAuthentication.ElaAuthenticationResponse) {}
  rpc Disconnect(ElaCommon.ElaInputBaseRequest) returns (ElaCommon.ElaError) {}
}
```

To explain how the authentication works, the schematics below explain how to use the connect and disconnect function to access to the function from the API. Between a connect and disconnect, your session token is available and you can use it to access to all the function. Otherwise, after disconnection or before connection, you cannot access to the functions.
<p align="center">
  <img width="650" src="https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Authentication_Work_01.png">
</p>

If you don't use the nuget provided by ELA Innocation, you need to implement the authentication mecanisms on your side to handle the target session token to fulfill in your API call. To understand how it works, you can follow this link **TODO_MAT**

[here_wirepas_github]: https://github.com/wirepas
