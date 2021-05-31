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

In the **ElaAuthentication.ElaAuthenticationRequest** object, you have to define two paremeters to fulfill a authentication. The first one is your **login**, the second one is your **certificate** it means your password in this case. The third object is the request (**ElaCommon.ElaInputBaseRequest**) and before to go deeper in the description you have to know that for each remote call using the interface we provide, you have to define this objet and define the right values at each times. So what are the different values handle in this object.
```proto
/*
 * \class ElaInputRequest
 * \brief Base request 
 */
message ElaInputBaseRequest {

	string client_id = 1;
	string session_id = 2;
	string request_id = 3;
	string client_ip_address = 4; // reserved parameter constructor
}
```

Parameters **client_ip_address** is a reserved parameter and not usefull when you use the microservice suite. 
Parameters **client_id** and **request_id** and are usefull for microservice and to build the answer. You should use this parameter to notify to the microservice who is talking with him (stand for **client_id**) and identify the request (stand for **request_id**). It is your responsibility to fill in these parameters, they will help you on client side to identify and ensure that the answer from the service is the one expected.
Parameter **session_id** is not necessary to initiate the connection, but for the orher call you have to fill this parameter with the target **session id** returned from the result of the **Connect** function.

The follwing sample show you how to initialte a connection with the **Connect** function from the Bluetooth module (the operating mode for the Wirepas module is the same).
```C#
	// Create the input request
	ElaInputBaseRequest elaInputBase = new ElaInputBaseRequest() {
		client_id = "My_Client_Id_01,
		session_id = String.Empty,
		request_id = "Request_0000001",
		client_ip_address = String.empty
	};
	// Create the input request
	ElaInputBaseRequest inputRequest = new ElaInputBaseRequest() {
		request = elaInputBase,
		login = "<my_ela_login>",
		certificate = "<my_ela_password>"
	};
```

Here we are, we create the base request to connect to the **Bluetooth Base Module** and try to authenticate. Note that for the **login** and **password**, you have to replace **"<my_ela_login>"** and **"<my_ela_password>"** by your own parameters. Now with the fill object, we can call the Connect function:
```C#
	//
	// [...]
	//
	String strInternalSessionId = String.Empty;
	//
	// create a client for bluetooth base service
	ElaBluetoothPublicService.ElaBluetoothPublicServiceClient bleClient = new ElaBluetoothPublicService.ElaBluetoothPublicServiceClient(new Channel("127.0.0.1", 50051, ChannelCredentials.Insecure));
	ElaAuthenticationResponse response = bleClient.Connect(inputRequest);
        if(ErrorServiceHandlerBase.ERR_OK == response.Error.Error)
        {
		//
		// There ! is the connection SUCCESS, your session Id is available
		strInternalSessionId = response.SessionId;
	}
	//
	// [...]
	//
```

Now, you need to store your **session Id** like we do with **strInternalSessionId** parameter. Then you can use this **session id** to call the other function from the API. The exemple bleow show you how we can **Start the scanner**, **Get the results** and **Stop the scanner**.
```C#
	//
	// [...]
	//
	ElaBluetoothScanningRequest request = new ElaBluetoothScanningRequest();
	request.Request = new ElaInputBaseRequest() {
		client_id = "My_Client_Id_01,
		session_id = strInternalSessionId,
		request_id = "Request_0000002",
		client_ip_address = String.empty
	};
	//
	ElaError error = bleClient.StartBluetoothListening(request);
	//
	// [...]
	//
```
