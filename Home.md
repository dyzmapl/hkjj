# Overview
UgCS .Net SDK gives access to core services of our Universal Control Server starting from version 2.10. Using this SDK it is possible to develop third party client applications that can programmatically:  
•	access various UgCS entities like missions, routes, vehicles, vehicle profiles, payloads, telemetry, etc;  
•	subscribe to notifications like telemetry, vehicle connection, routes change, ads-b collision, etc;  
•	issue commands to vehicles;  
•	calculate routes;  
•	access elevation data.  

SDK gives developer a way to implement a simple widget to display telemetry parameter or full functional drone control client application.

# Getting started
## Creating project
Open Visual Studio and select on of possible C# projects. To use SDK you have to add references to appropriate assemblies. The most recent assemblies are available in our nugget repository. To get package open your package manager command line and type: 
`nuget install ugcs-dotnet-sdk`

After successful download the following references shall appear in your project:
-	Sdk.dll
-	Protobuf-net.dll
We use protobuf as a container for our messages so SDK uses it as a dependency.
Add the following namespaces to your “using” sections of C# code:

`using Hci.Sdk;`  
`using Hci.Sdk.Protocol; `  
`using Hci.Sdk.Protocol.Encoding;  `  
`using Hci.Sdk.Tasks; `  

## Basic concepts
There are two ways of communication between client application and UGCS Server:
-	asynchronous request/ response;
-	subscription.
Though all communication is asynchronous there are certain ways to implement synchronous workflow on the client.
Typically you will use requests/responses to make standard CRUD operations and to list objects. For example get list of vehicles, create vehicle profile, remove payload, etc.
Subscriptions are useful to listen for some events. For example if you want to get telemetry stream for the vehicle you typically subscribe to telemetry events. It is more appropriate way comparing to setting timer on the client and pulling UCS every N seconds.  
 
## Connecting to UGCS Server
Establishing connection to UGCS Server is the first step to be performed. It consists of the following main operations:  
1. establishing TCP connection;  
2. authorizing client session.  

Create instance of TcpClient class and pass host and port as parameters. If UgCS Server is running on the same computer then specify “localhost” for a host and 3334 as a port number.

`TcpClient client = new TcpClient(host, port);`  

To test your application you need UgCS Server running. Check that clicking UgCS icon in tray.
 
Compile and run that simple application. If client object instantiated successfully than you may find your session object in client.Session property.

The next step is to get clientID. ClientID is a thing that you must specify for every request towards UgCS Server. But before that we need to prepare some code for sending and receiving messages.

            MessageFuture<AuthorizeHciResponse> authFuture =  
                executor.Submit<AuthorizeHciResponse>(new AuthorizeHciRequest  
            {  
                ClientId = -1,  
                Locale = "en-US",  
            }  
            );  
            AuthorizeHciResponse authResp = authFuture.Value;  
            int clientID = authResp.ClientId;  


What we do here is just sending AuthorizeHciRequest to UgCS Server. Request has to parameters: 
* ClientID and we set it to default -1 value;
* Locale – client locale, en-US by default here. Local defines localization for strings that UgCS Server sends to client.

Another important thing to note is how we get response from the UgCS Server. We employ future (or promise) concept here for asynchronous communication.  Typically this means that you submit some response , specify callback and do not wait for response doing something. But here we explicitly wait for response invoking Value property of authFuture object. That is a general approach for making pseudo-synchronous requests/responses.    
We are almost ready to do something really useful.  The only thing that has to be done is to login into the system. UgCS Server uses login based authentication model. Look at the source code:

             MessageFuture<LoginResponse> loginFuture =  
                executor.Submit<LoginResponse>(new LoginRequest  
             {  
                    ClientId = clientId,  
                    UserLogin = login,  
                    UserPassword = password  
            });  
            LoginResponse loginResp = loginFuture.Value; 

 
It is pretty straightforward. Note that we use clientID as an argument to LoginRequest. The only question is how to deal with login and password. Generally you must specify login and password from user list. But there is a trick. If you have only one user registered then you can put empty strings and SDK will try to make autologin. 
That’s it. Now we are logged in into UGCS server and can do something.

## Accessing server objects
### Vehicles
**List**
This example shows how to get vehicle list from the server. First of all we need find appropriate request/response pair. There is a universal operation for getting object lists called GetObjectList. It works similar for many objects. You need to specify clientId and ObjectType. In our case object type is “Vehicle”

            MessageFuture<GetObjectListResponse> vehicleFuture =  
                executor.Submit<GetObjectListResponse>(  
                new GetObjectListRequest  
                {  
                    ClientId = clientId,  
                    ObjectType = "Vehicle"  
                });  
            GetObjectListResponse vehicleListResp = vehicleFuture.Value;  
 
Nothing new in this sample. And we did a half of the job. Now we need to access vehicle data itself. Response object contains a lot of general properties and some specific ones. Actual data resides in Objects collection. So to list vehicles we can use the following code:

            foreach (DomainObjectWrapper v in vehicleListResp.Objects)  
            {  
                Console.WriteLine(string.Format("name: {0}; id: {1}; type: {2}",  
                    v.Vehicle.Name, v.Vehicle.Id, v.Vehicle.Type.ToString()));  
            }  

**Update**
This example shows how to update vehicle on the server.

    CreateOrUpdateObjectRequest request = new CreateOrUpdateObjectRequest()  
    {  
        ClientId = clientId,  
        Object = new DomainObjectWrapper().Put(vehicle, "Vehicle"),  
        WithComposites = true,  
        ObjectType = "Vehicle",  
        AcquireLock = false  
    };  
    var task = executor.Submit<CreateOrUpdateObjectResponse>(request);  

**Delete**
This example shows how to delete vehicle on the server.

    DeleteObjectRequest request = new DeleteObjectRequest()  
    {  
        ClientId = clientId,  
        ObjectId = vehicle.Id,  
        ObjectType = "Vehicle"  
    };  
    var task = _connect.Executor.Submit<DeleteObjectResponse>(request);    

### Vehicle profile
**List**
    MessageFuture<GetObjectListResponse> listFuture =  
        executor.Submit<GetObjectListResponse>(  
        new GetObjectListRequest  
        {  
            ClientId = clientId,  
            ObjectType = "VehicleProfile"  
        });  
  
   GetObjectListResponse listResp = listFuture.Value;  

Response object contains data in Objects property. Each object is of DomainObjectWrapper type. Wrapper has a lot of properties for different object types. In this case we need VehicleProfile property. Each profile stores it’s parameters in Parameters collection. 

**Create**
TODO

**Update**
TODO

**Delete**
TODO

###Payloads
**List**
    MessageFuture<GetObjectListResponse> listFuture =  
        executor.Submit<GetObjectListResponse>(  
        new GetObjectListRequest  
        {  
             ClientId = clientId,  
             ObjectType = "PayloadProfile"  
        });  

    GetObjectListResponse listResp = listFuture.Value;  

Response object contains data in Objects property. Each object is of DomainObjectWrapper type. Wrapper has a lot of properties for different object types. In this case we need PayloadProfile property. Each profile stores it’s parameters in Parameters collection. 

**Create**
TODO

**Update**
TODO

**Delete**
TODO

### Missions
**Create mission**

    Mission mission = new Mission  
    {  
        CreationTime = 1270011748,  
        Name = "Mission name",  
        Owner = user  
    };  

    CreateOrUpdateObjectRequest request = new CreateOrUpdateObjectRequest()  
    {  
        ClientId = 1,  
        Object = new DomainObjectWrapper().Put(mission, "Mission"),  
        WithComposites = true,  
        ObjectType = "Mission",  
        AcquireLock = false  
    };  
    var task = executor.Submit<CreateOrUpdateObjectResponse>(request);  

## Accessing telemetry storage
Here is a sample of telemetry request to obtain telemetry for the last hour. Not that we specify vehicle object (see Vehicles sample) as a parameter. Another important parameters are ToTime and FromTime – time interval.  Limit defines maximum number of telemetry records to be fetched. Zero means no limit.

    MessageFuture<GetTelemetryResponse> telemetryFuture = executor  
        .Submit<GetTelemetryResponse>(new GetTelemetryRequest  
    {  
        ClientId = authResp.ClientId,  
        Vehicle = vehicleListResp.Objects[0].Vehicle,  
        ToTime = DateTimeUtilities.ToPosixMilliseconds(DateTime.Now),  
        FromTime =  DateTimeUtilities.ToPosixMilliseconds(  
            DateTime.Now.Subtract(new TimeSpan(1,0,0))),  
        Limit = 0,  
        LimitSpecified = true,  
        ToTimeSpecified = true  
    });  

    GetTelemetryResponse telemetryResp = telemetryFuture.Value;  

Response object contains data in Telemetry property. Each object is of TelemetryDto type. Field “Type” contains field type, “Value” contains value and “Time” is a time.

One more important things to note is that in the sample above we use Posix time. In the future releases of SDK we will include these functions in our SDK. But you can use the following code instead for now:
    public static long ToPosixMilliseconds(this DateTime localTime)  
    {  
        DateTime utcTime = localTime.ToUniversalTime();  
        TimeSpan span = utcTime - PosixEpoch;  
        return (long)span.TotalMilliseconds;  
    }  

    public static DateTime FromPosixMilliseconds(long milliseconds)  
    {  
        TimeSpan span = TimeSpan.FromMilliseconds(milliseconds);  
        return (PosixEpoch + span).ToLocalTime();  
    }  

Note: telemetry recording is disabled in UgCS for Emulator by default. For development purposes is convenient to switch it on. To do that open <UgCS Installation Dir>\Server\ucs\ucs.properties and change field ucs.emulator.storeTelemetry=true.


