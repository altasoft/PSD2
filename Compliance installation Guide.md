# Alta Software B7.Compliance Installation Guide for IIS

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## Install SQL Server Express (2016+) or you can use existing SQL server (Enterprise)
    1. Download the installer using the following link:
        [Latest SQL Server Express installer direct download](https://go.microsoft.com/fwlink/?linkid=866658)
        
        If you already have SQL server installed somewhere, you can use it.

    1. Run SQL Server Management Studio
    1. Connect to SQL Server
    1. Open new query window
    1. Create database with the following command:

    ```sql
    CREATE DATABASE [B7.Compliance] COLLATE Latin1_General_100_BIN2;
    ```
1. ## Install Rabbit MQ  Or use existing Rabbit MQ.  [Direct download] (https://www.rabbitmq.com/download.html)
    1.  With RabbitMQ Ui create new virtualHost B7.Compliance

1. ## Download latest versions of B7.Compliance binaries. (Api, EventHandler, EventPublisher,Migration)
    You can download it from our support portal [Alta Software Support Site](https://jira.altasoft.ge/).(Please Contact support for further details regarding Downloading binaries)

1. ## Extract B7.Compliance aplications in IIS folders
    
    1. Create folder for B7.Compliance applications, i.e. ```C:\Inetpub\B7.Compliance```
  
    1. Extract B7.Compliance.Api-x.x.x.zip to ```C:\Inetpub\B7.Compliance\Api``` folder
  
    1. Extract B7.Compliance.EventHandler-x.x.x.zip to ```C:\Inetpub\B7.Compliance\EventHandler``` folder
  
    1. Extract B7.Compliance.EventPublisher-x.x.x.zip to ```C:\Inetpub\B7.Compliance\EventPublisher``` folder
  
    1. Extract B7.Compliance.Migration-x.x.x.zip to ```C:\Inetpub\B7.Compliance\Migration``` folder
  
1. ## Fill Appsettings.json 
    1. # Api
        
        1. Fill ```Database``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```B7.Compliance``` database.
            ```json
            "Database": {
               "ConnectionString": "Server=docker2;Database=B7_Compliance;User Id=XX;Password=XXX;Application Name=B7.Compliance.Api;TrustServerCertificate=true",
             "DbSchemaName": "compliance"
            },
            ```
        1. Fill  ```FircoOptions```  section with the values provider by Firco.
            ```json
            "FircoOptions": {
                "Address": "https://amlpre.crystal.ge",
                "ClientId": "rest",
                "ClientSecret": "Rest.Password1",
                "AccountId": 1
             },
            ```
        1. Fill ```CoreApiOptions``` with the address and userId providet to access Core Api services
            ```json
             "CoreApiOptions": {
            "Address": "http://iisdev.altasoft.local:3131",
            "UserId": 30170
            },
            ```
        1. Fill ```AppEnvironment``` With the environment with one of the values: Testing, Production  
            ```json
            "AppEnvironment": {
            "Type": "Testing"
            },
            ```
        1. Fill ```EventPublisherBus``` Section with the values for rabbitMqConnection. `Do not modify TopicName`
            ```json
            "EventPublisherBus": {
            "ConnectionString": "HostName=docker2;VirtualHost=B7.Compliance;UserName=guest;Password=guest;ClientProvidedName=B7.Compliance.Api",
            "TopicName": "B7.Compliance.Events"
            },
            ```
        1. If you are using ServiceDiscovery fill the parameters for the section ```ServiceDiscovery``` otherwise set value of ```AutoRegister``` to false
            ```json
              "ServiceDiscovery": {
             "AutoRegister": true,
             "Type": "ServiceMaster",
             "Address": "http://iisdev2:23357",
             "Token": "398073a8-5091-4d9c-871a-bbbeb030d1f6",
             "HealthCheckPath": "health",
             "ServiceAddress": "http://iisdev:8096",
             "ServicePort": 8096
            }
            ```
    1. # EventHandler    
        1. Fill  ```FircoOptions```  section with the values provider by Firco.
            ```json
            "FircoOptions": {
                "Address": "https://amlpre.crystal.ge",
                "ClientId": "rest",
                "ClientSecret": "Rest.Password1",
                "AccountId": 1
             },
            ```
        1. Fill ```CoreApiOptions``` with the address and userId providet to access Core Api services
            ```json
             "CoreApiOptions": {
            "Address": "http://iisdev.altasoft.local:3131",
            "UserId": 30170
            },
            ```
               
        1. Fill ```AppEnvironment``` With the environment with one of the values: Testing, Production  
            ```json
            "AppEnvironment": {
            "Type": "Testing"
             },
            ```
        1. Fill ```QueueProviders``` ``Default`` with B7.Compliance RabbitMq virualHost and ```B6``` with B6 VirtualHost
            ```json
            "QueueProviders": {
            "Default": {
            "ConnectionString": "HostName=docker2;VirtualHost=B7.Compliance;UserName=guest;Password=guest;ClientProvidedName=B7.Compliance.EventHandler",
            "CheckConsumersInterval": 2000,
            "HandleMessageMaxAttempts": 3,
            "HandleMessageInterval": 1500
            },
            "B6": {
            "ConnectionString": "HostName=docker2;VirtualHost=B6;UserName=guest;Password=guest;ClientProvidedName=B7.Compliance.EventHandler",
            "CheckConsumersInterval": 2000,
            "HandleMessageMaxAttempts": 3,
            "HandleMessageInterval": 1500
            }
            },
            ```
         1. Fill ```Database``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```B7.Compliance``` database.
            ```json
            "Database": {
               "ConnectionString": "Server=docker2;Database=B7_Compliance;User Id=XX;Password=XXX;Application Name=B7.Compliance.Api;TrustServerCertificate=true",
             "DbSchemaName": "compliance"
            },
            ```
    1. # EventPublisher 
         1. Fill ```AppEnvironment``` With the environment with one of the values: Testing, Production  
            ```json
            "AppEnvironment": {
            "Type": "Testing"
             },
            ```
         1. Fill ```EventPublisherBus``` Section with the values for rabbitMqConnection. `Do not modify TopicName`
            ```json
            "EventPublisherBus": {
            "ConnectionString": "HostName=docker2;VirtualHost=B7.Compliance;UserName=guest;Password=guest;ClientProvidedName=B7.Compliance.Api",
            "TopicName": "B7.Compliance.Events"
            },
            ```
          1. Fill ```Database``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```B7.Compliance``` database.
            ```json
            "Database": {
               "ConnectionString": "Server=docker2;Database=B7_Compliance;User Id=XX;Password=XXX;Application Name=B7.Compliance.Api;TrustServerCertificate=true",
             "DbSchemaName": "compliance"
            },
            ```
    1. # Migration 
        1. Fill ```Settings``` section. 
        
            ```IsCustomerProfileActive``` and ```IsTransactionProfileActive``` must be ```true``` and optionally set value to true for ```IsMonitoredCustomerActive``` if you need to migrate customer from B6.

            Fill ```FircoXXX``` with parameters provided by the provider.

            Fill ```B6ConnectionString``` with the correct B6 Connection string
            you can leave DegreeOfParallelism unchanged. 

            ```json
            "Settings": {
            "DegreeOfParallelism": 8,
            "IsCustomerProfileActive": true,
            "IsTransactionProfileActive": true,
            "IsMonitoredCustomerActive": true,
            "FircoCustomerProfileId": "3c960bb4-a51a-465c-b1aa-5b1efadc36e5",
            "FircoCustomerProfileDatasetId": 2,
            "FircoTransactionProfileId": "3c960bb4-a51a-465c-b1aa-5b1efadc36e5",
            "FircoTransactionProfileDatasetId": 2,
            "B6ConnectionString": "Data Source=192.168.1.1;Initial Catalog=BANK2000;Connection Timeout=30;User Id=user;Password=password;Application Name=B7.CTPM.Migration;Encrypt=false;TrustServerCertificate=true;"
            },
            ```
        1. Fill ```AppEnvironment``` With the environment with one of the values: Testing, Production  
            ```json
            "AppEnvironment": {
            "Type": "Testing"
             },
            ```
        
        1. Fill ```Database``` section with database connection string.
            ```json
            "Database": {
               "ConnectionString": "Server=docker2;Database=B7_Compliance;User Id=XX;Password=XXX;Application Name=B7.Compliance.Api;TrustServerCertificate=true",
             "DbSchemaName": "compliance"
            },
            ```



1. ## Install B7.Compliance.Api in IIS

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pool
        
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```B7.Compliance.Api_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

    1. Create Web Sites

        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```B7.Compliance.Api``` into ```Site name``` field
        1. Select ```B7.Compliance.Api_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\B7.Compliance\Api``` into ```Physical path``` field
        1. Select ```https``` or ```http``` in ```Binding: Type``` field
        1. optionally add server name identification  ```compliance.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
            1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
            1. Press ```OK```
        1. Select newly created site, right click it and select ```Edit Bindings...```
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```


1. ## Run Migration
    1. Go To `C:\Inetpub\B7.Compliance\Migration` and run `B7.Compliance.Migration.exe` file. If customer migration is enabled the process will take a long time otherwise it should be finished within seconds. Wait until it's finished before running EventHandler.


1. ## Install EventHandler and EventPublisher as WindowsService  `After Migration is finalized!`

    1. Using powershell or Windows command line run the following scripts 

        ```sc.exe create "B7.Compliance.EventHandler" binpath="C:\Inetpub\B7.Compliance\EventHandler\B7.Compliance.EventHandler.exe" ```
       ```sc start B7.Compliance.EventHandler```

    1. Using powershell or Windows command line run the following scripts 

        ```sc.exe create "B7.Compliance.EventPublisher" binpath="C:\Inetpub\B7.Compliance\EventPublisher\B7.Compliance.EventPublisher.exe" ```
       ```sc start B7.Compliance.EventPublisher```

    you can also set auto run to true from windows services, or failure options (for more details on how to run windows service  use hte link: https://learn.microsoft.com/en-us/dotnet/core/extensions/windows-service).




1.  That's it. :smiley:
    * Check that everything is working as expected