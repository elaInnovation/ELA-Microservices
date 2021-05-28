# Deal With Authentication

This documentation try to explain how do deal with the authentication module and get a valid session Id (your authentication token) to access to all the function provided by an ELA API. So, in this paragraph, we will explain the purpose with some example, some code and shematics. The Authentication module is necessessary to use **ELA Microservices**, without it you cannot use the API.

So if you are ready, we will deep into the explanation.

## Description
Every service has to function to established an authenticate connection. As we can see in the proto declaration just below, you will find the two function **Connect** and **Disconnect**. (***The proto declaration is a part from the Bluetooth Base Microservice API***)
```proto
// The BackendConfig service definition.
service ElaBluetoothPublicService {

  // authentication functions
  rpc Connect(ElaAuthentication.ElaAuthenticationRequest) returns (ElaAuthentication.ElaAuthenticationResponse) {}
  rpc Disconnect(ElaCommon.ElaInputBaseRequest) returns (ElaCommon.ElaError) {}
}
```

- **Connect** function : by using the input parameters (***ElaAuthenticationRequest***), you try to connect to the module using your ***login*** and ***password*** defined for your current deployement. This function return a response (***ElaAuthenticationResponse***) with your connection informations.
- **Disconnect** function : this function disconnect from the module and release the session Id you got after connection

The schematics below explain how to use the connect and disconnect function to access to the function from the API. Between a connect and disconnect, your **session Id** is available and you can use it to access to all the function. Otherwise, after disconnection or before connection, you cannot access to the functions.
<p align="center">
  <img width="650" src="https://github.com/elaInnovation/ELA-Microservices/blob/master/Images/ELA_Authentication_Work_01.png">
</p>

So, it's time to go deeper by having a look with all the object used in this interface. The first object is the common message **ElaAuthentication.ElaAuthenticationRequest**. This one is a generic parameter for all function provided in the public API.
```proto
/*
 * \class ElaAuthenticationRequest
 * \brief information relative the authentication functions
 */
message ElaAuthenticationRequest {

	ElaCommon.ElaInputBaseRequest request = 1;
	string login = 2;
	string certificate = 3;
}
```

In the **ElaAuthentication.ElaAuthenticationRequest** object, you have two define two paremeters to fulfill a authentication. The first one is your **login**, the second one is your **certificate** it means your password in this case. The third object is the request (**ElaCommon.ElaInputBaseRequest**) and before to go deeper in the description you have to know that for each remote call using the interface we provide, you have to define this objet and define the right values at each times. So what are the different values handle in this object.
```proto
/*
 * \class ElaInputRequest
 * \brief Base request 
 */
message ElaInputBaseRequest {

	string client_id = 1;
	string session_id = 2;
	string request_id = 3;
	string client_ip_address = 4;
}
```

The parameters **client_id**, **request_id** and **client_ip_address** ... TO_BE_CONTINUED