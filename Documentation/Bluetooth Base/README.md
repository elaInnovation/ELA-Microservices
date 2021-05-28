# ELA Bluetooth Base Microservice

Here, you will find all the informations relatives to the **Bluetooth Base Microservice** developped by ELA Innovation. Once the installation complete, this microservice allow you to use the bluetooth layer from your device to release simple functions like scanning or connecting to an **ELA Blue Device**.

## Service Description
We implement on server side a service which deal with the bluetooth layer of your computer to scan and connect **ELA Blue Device**. The main functionnalities we expose through our API allow a user to:
- Start the bluetooth scanner
- Stop the bluetooth scanner
- Get all the results from a scan
- Connect to an **ELA Blue Device** to send a specific command
- Get the current state from the microservice
- Define a filter for the bluetooth scanner
- Release the filter for the bluetooth scanning

We keep all this functionnalities as simple as possible. The API as only the following function:
- **StartBluetoothListening** : to start the bluetooth scanner and define a filter (as function parameter)
- **StopBluetoothListening** : to stop a current instance of scanning (engaged with StartBluetoothListening)
- **GetScannedDevices** : to get all the scanning data catched by the bluetooth scanner
- **SendElaBluetoothCommand** : to send a bluetooth command and get the result from the tag
- **GetServiceStatus**: to get the service information (version, os, state ...)
```proto
// The service definition.
service ElaBluetoothPublicService {

  // bluetooth scanning
  rpc StartBluetoothListening(ElaBluetoothScanningRequest) returns (ElaCommon.ElaError) {}
  rpc GetScannedDevices(ElaCommon.ElaInputBaseRequest) returns (ElaBluetoothScanResults) {}
  rpc StopBluetoothListening(ElaCommon.ElaInputBaseRequest) returns (ElaCommon.ElaError) {}

  // bluetooth connections
  rpc SendElaBluetoothCommand(ElaBluetoothConnectRequest) returns (ElaBluetoothConnectResult) {}

  // monitoring
  rpc GetServiceStatus(ElaCommon.ElaInputBaseRequest) returns (ElaBluetoothMicroserviceStatus) {}
}
```
You can find all the information about the API and object here.

Through the function available in the API, we can describe the state machine associated to the **Bluetooth Base Microservice**. 
<p align="center">
  <img width="650" src="https://github.com/elaInnovation/ELA-Microservices/blob/master/Documentation/Bluetooth%20Base/Images/Bluetooth_StateMachine_01.png">
</p>
  
The following schematics describe all the state:
- **Pending**: This is the initial state of the service, when no action has been triggered the service is in Pending State.
- **Scanning**: The scanner switch in scanning mode when **StartBluetoothListening** function called. The service leave this state if **StopBluetoothListening** called or when a connection is trigerred.
- **Connecting**: a connection request has been received and the service try to connect to the target tag. The state is not connected while the tag and the service have no connection established.
- **Connected**: a connection between the tag and the service has been established. While this state is maintained, the dialog is open between tag and service, try to send command and receive a response

## Authentication
To use the different function from the API, you need use the authentication function provided in the Bluetooth Base API. Two function are here to do the authentication job:
- Connect: provide your login and password information and get a session token to use all the functionnalities provided by Bluetooth Base API
- Disconnect: diconnect your client from the bluetooth module and delete the availability of your session token
```proto
// The BackendConfig service definition.
service ElaBluetoothPublicService {

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

