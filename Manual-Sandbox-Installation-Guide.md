# Alta Software PSD2 Developer Portal and Sandbox Manual Installation Guide for IIS

1. ## Install IIS 10

1. ## Delete all non root certificates from ```Trusted root certificates```
    You should delete any certificate whose ```Issued by``` and ```Issued to``` values are not the same (and therefore the certificate is not at the top of the hierarchy).
    
    Plese see this: https://docs.microsoft.com/en-us/troubleshoot/iis/http-403-forbidden-open-webpage

1. ## Disable weak protocols, cipher suites and hashing algorithms
    Please read this: 
        [Transport Layer Security (TLS) registry settings](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings)

    and this
        [Manage Transport Layer Security (TLS)](https://docs.microsoft.com/en-us/windows-server/security/tls/manage-tls)

1. ## Install Banking Associacion of Georgia-s root certificate in ```Trusted Root Certification Authorities``` with store location = ```Local Machine```.

1. ## Install Banking Associacion of Georgia-s sub root certificates (WAC and SEAL) in ```Intermediary Certification Authorities``` with store location = ```Local Machine```.

1. ## Install your SSL certificate (i.e. ```*.yourdomain.ge```) in ```Personal``` with store location = ```Local Machine```. 

1. ## Install your QWAC compatible certificate (i.e. ```*.yourdomain.ge```) in ```Personal``` with store location = ```Local Machine```. 

1. ## Install your QSealC compatible certificate in ```Personal``` with store location = ```Local Machine```. It MUST have private key.

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## (Conditional) Install SQL Server Express (2016+) or you can use existing SQL server (Enterprise)
    1. If you are using PSD2 DataAccess Api, then skip this step and install DataAccess Api instead [DataAccess Installation Guide](https://github.com/altasoft/PSD2/blob/main/DataAccess-Installation-Guide.md)
    
    1. Download the installer using the following link:
        [Latest SQL Server Express installer direct download](https://go.microsoft.com/fwlink/?linkid=866658)
    
        If you already have SQL server installed somewhere, you can use it.

    1. Run SQL Server Management Studio
    1. Connect to SQL Server
    1. Open new query window
    1. Create database with the following command:

    ```sql
    CREATE DATABASE [PSD2_Portal] COLLATE Latin1_General_100_BIN2;
    ```

1. ## Download latest versions of PSD2 Developer Portal and Sandbox binaries

    You can download it from our support portal [Alta Software Support Site](https://jira.altasoft.ge/). Go to PSD2 folder.

1. ## Extract PSD2 Developer Portal and Sandbox aplications in IIS folders
    
    1. Create folder for PSD2 sandbox applications, i.e. ```C:\Inetpub\PSD2```
  
    1. Extract sandbox-portal-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\portal``` folder
  
    1. Extract sandbox-auth-server-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\authserver-sandbox``` folder
  
    1. Extract sandbox-auth-web-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\authweb-sandbox``` folder
  
    1. Extract sandbox-xs2a-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\xs2a-sandbox``` folder
  
    1. Go to each folder, find ```appsettings.json``` file, open it and:
        1. If you are ***not using*** PSD2 DataAccess Api, then fill ```Database:ConnectionString``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```PSD2_Portal``` database. Delete ```DataAccess``` section, it is only required when using PSD2 DataAccess Api.
            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2_Portal;Integrated Security=true;Application Name=AltaSoft.PSD2"
            }
            ```

        1. If you are ***using*** PSD2 DataAccess Api, then fill ```DataAccess``` section with address of installed DataAccess Api. Fill ```UserName``` and ```Password``` with credentials entered in PSD2 DataAccess Api configuration. Delete ```Database:ConnectionString``` section, it is only required when using direct access to PSD2 database.
            ```json
            "DataAccess": {
                "Url": "https://localhost:15011",
                "UserName": "User",
                "Password": "Password"
            }
            ```

        1. Fill ```"CertificateThumbprint``` with your QSealC certificate's thumbprint
            ```json
            "CertificateThumbprint": "9c3f0b85333b72379963e610e1d95c94d4fa5166"
            ```
   
        2. If you ```have``` load balancer in front of IIS
            ```json
            "HostingType": "load_balancer"
            ```
            
            You should configure your Load Balancer for MTLS and certificate forwarding in http header with name ```X-ARR-ClientCert```
            
    1. Open ```C:\Inetpub\PSD2\xs2a-sandbox\appsettings.json``` and fill ```OAuth2ServerBaseUrl``` parameter with this ```https://psd2-authserver-sandbox.yourdomain.ge```
        ```json
        "OAuth2ServerBaseUrl": "https://psd2-authserver-sandbox.yourdomain.ge"
        ```

    1. Open ```C:\Inetpub\PSD2\authserver-sandbox\appsettings.json``` and fill ```OAuth2WebBaseUrl ``` parameter with this ```https://psd2-authweb-sandbox.yourdomain.ge```
        ```json
        "OAuth2WebBaseUrl": "https://psd2-authweb-sandbox.yourdomain.ge"
        ```

1. ## Install PSD2 Developer Portal and Sandbox aplications in IIS

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pools
        
        ![Image](../main/Images/EditAppPool.png)
        
        ![Image](../main/Images/AppPoolAdvanced.png)

        #### Developer Portal
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.DeveloperPortal_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

        #### OAuth2 Server API
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.AuthServer.Sandbox_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

        #### OAuth2 Server Web
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.AuthWeb.Sandbox_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

        #### XS2A API
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.XS2A.Sandbox_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

    1. Create Web Sites

         ![Image](../main/Images/AddWebSite.png)

        #### Developer Portal
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.DeveloperPortal``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.DeveloperPortal_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\portal``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```psd2-portal.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Select newly created site, right click it and select ```Edit Bindings...```
        1. Select ```http``` in ```Type``` field
        1. Enter ```psd2-portal.yourdomain.ge``` in ```Host name``` field
        1. Press ```OK```
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```

        #### OAuth2 Server API
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.AuthServer.Sandbox``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.AuthServer.Sandbox_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\authserver-sandbox``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```psd2-authserver-sandbox.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Check ```Disable HTTP/2```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Select newly created site and select ```SSL Settings```
        
            ![Image](../main/Images/SslSettings.png)

            1. Select ```Require SSL```
            1. Select ```Accept``` in ```Client certificates```
        1. Press ```Apply``` button
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```

            
        #### OAuth2 Server Web
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.AuthWeb.Sandbox``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.AuthWeb.Sandbox_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\authweb-sandbox``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```psd2-authweb-sandbox.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```


        #### XS2A API
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.XS2A.Sandbox``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.XS2A.Sandbox_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\xs2a-sandbox``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```psd2-xs2a-sandbox.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Check ```Disable HTTP/2```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Select newly created site and select ```SSL Settings```
        
            ![Image](../main/Images/SslSettings.png)

            1. Select ```Require SSL```
            1. Select ```Accept``` in ```Client certificates```
        1. Press ```Apply``` button
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```


<!-- 1.  ## !!! Temporary, until real certificates will be available !!!
    Go to each folder (except authweb-sandbox), find ```appsettings.json``` file, open it and and fill ```RootCertificateThumbprints``` parameter with thumbprints of all PSD2 root certificates.
        
    Include your root PSD2 certificate (Generated according to [Banking association of Georgia document](https://bitbucket.org/obgeo/development-ca/src/master/))
        
    Please also include temporarily Alta Sofwate PSD2 root certificate thumprint ```F9AA2ABAD10F448F7F3759AA4B3798ED19A0CCDD``` for testing purposes.
        
    Separate thumprints with ```,``` (comma) character

    ```json
    "RootCertificateThumbprints": "e06aed78542ee8a994c229489c6e31909e7e6441,F9AA2ABAD10F448F7F3759AA4B3798ED19A0CCDD"
    ``` -->

1.  That's it. :smiley:
    * Check that everything is working as expected
