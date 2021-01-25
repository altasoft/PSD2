# Alta Software PSD2 Developer Portal and Sandbox Manual Installation Guide for IIS

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## Install SQL Server Express (2016+)

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

    You can download it from our support portal 
     [Alta Software Support Site](https://jira.altasoft.ge/)

1. ## Extract PSD2 Developer Portal and Sandbox aplications in IIS folders
    
    1. Create folder for PSD2 sandbox applications, i.e. ```C:\Inetpub\PSD2```
    1. Extract sandbox-portal-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\portal``` folder
    1. Extract sandbox-auth-server-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\authserver-sandbox``` folder
    1. Extract sandbox-auth-web-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\authweb-sandbox``` folder
    1. Extract sandbox-xs2a-BBBBBBBB-x.x.x.zip to ```C:\Inetpub\PSD2\xs2a-sandbox``` folder
    1. Go to each folder, find ```appsettings.json``` file, open it and fill ```Database:ConnectionString``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```PSD2_Portal``` database
    ```json
    "Database": {
        "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2_Portal;Integrated Security=true;Application Name=AltaSoft.PSD2"
    }
    ```
    1. Open ```C:\Inetpub\PSD2\portal\appsettings.json``` and fill ```OAuth2ServerBaseUrl``` parameter with this ```https://authserver-sandbox-psd2.yourdomain.ge```
    ```json
    "OAuth2ServerBaseUrl": "https://authserver-sandbox-psd2.yourdomain.ge"
    ```



    1. Go to each folder, find hosting.json file, open it and write this:

    ```json
    "HostingType": "Iis",
    "InstanceId": 1
    ```

1. ## Install PSD2 Developer Portal and Sandbox aplications in IIS

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pools
        
        ![Image](../main/Images/EditAppPool.png)

        #### Developer Portal
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.DeveloperPortal_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field

        #### OAuth2 Server API
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.AuthServer_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field

        #### OAuth2 Server Web
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.AuthWeb_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field

        #### XS2A API
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.XS2A_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field

    1. Create Web Sites

         ![Image](../main/Images/AddWebSite.png)

        #### Developer Portal
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.DeveloperPortal``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.DeveloperPortal_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\portal``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```portal-psd2.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Select newly created site, right click it and select ```Edit Bindings...```
        1. Select ```http``` in ```Type``` field
        1. Enter ```portal-psd2.yourdomain.ge``` in ```Host name``` field
        1. Press ```OK```

        #### OAuth2 Server API
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.AuthServer``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.AuthServer_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\authserver-sandbox``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```authserver-sandbox-psd2.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Select newly created site and select ```SSL Settings```
        
            ![Image](../main/Images/SslSettings.png)

            1. Select ```Require SSL```
            1. Select ```Require``` in ```Client certificates```
        1. Press ```Apply``` button
            
        #### OAuth2 Server Web
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.AuthWeb``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.AuthWeb_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\authweb-sandbox``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```authweb-sandbox-psd2.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```

        #### XS2A API
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.XS2A``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.xs2a-sandbox``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\xs2a-sandbox``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```xs2a-sandbox-psd2.yourdomain.ge``` in ```Binding: Host name``` field and select ```Require Server Name Indication```
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Select newly created site and select ```SSL Settings```
        
            ![Image](../main/Images/SslSettings.png)

            1. Select ```Require SSL```
            1. Select ```Require``` in ```Client certificates```
        1. Press ```Apply``` button

    1. Install altasoftPsd2RootCa.crt

        1. In File Explorer, goto ```C:\Inetpub\PSD2\portal\Certificates``` folder, locate file ```altasoftPsd2RootCa.crt```
        1. Right click it and press ```Install Certificate```
        1. In ```Store Location``` select ```Local Machine``` and press ```Next``` button
        1. Select ```Place all certificates in the following store``` and press ```Browse``` button
        1. Select ```Trusted Root Certification Authorities``` and press ```Next``` button
        1. Press ```OK``` button


    1.  That's it. :smiley:
        * Check that everything is working as expected
