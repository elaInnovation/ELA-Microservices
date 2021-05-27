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
// The BackendConfig service definition.
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
You can find all the information about he API and object here.

Through the function available in the API, we can describe the state machine associated to the **Bluetooth Base Microservice**. 
<p align="center">
  <img width="650" src="https://github.com/elaInnovation/ELA-Microservices/blob/master/Documentation/Bluetooth%20Base/Images/Bluetooth_StateMachine_01.png">
</p>
  
The following schematics describe all the state:
- **Pending**: This is the initial state of the service, when no action has been triggered the service is in Pending State.
- **Scanning**: The scanner switch in scanning mode when **StartBluetoothListening** function called. The service leave this state if **StopBluetoothListening** called or when a connection is trigerred.
- **Connecting**: a connection request has been received and the service try to connect to the target tag. The state is not connected while the tag and the service have no connection established.
- **Connected**: a connection between the tag and the service has been established. While this state is maintained, the dialog is open between tag and service, try to send command and receive a respon
