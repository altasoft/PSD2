# Alta Software PSD2 DataAccess Manual Installation Guide for IIS (Sandbox)

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## Install SQL Server (2016+) or you can use existing SQL server

    1. Run SQL Server Management Studio
    1. Connect to SQL Server
    1. Open new query window
    1. Create database with the following command:

    ```sql
    CREATE DATABASE [PSD2_Portal] COLLATE Latin1_General_100_BIN2;
    ```

1. ## Download latest version of PSD2 DataAccess binaries

    You can download it from our ftp site: [Direct download](https://psd2files.altasoft.ge/PSD2_DataAccess.zip)

1. ## Extract PSD2 DataAccess aplication in IIS folder
    
    1. Create folder for PSD2 applications, i.e. ```C:\Inetpub\PSD2```
    1. Extract PSD2_DataAccess.zip to ```C:\Inetpub\PSD2\DataAccess``` folder
    1. Go to folder, where you extracted ```PSD2_DataAccess.zip```, find ```appsettings.json``` file, open it and:
    
        1. Fill ```Database:ConnectionString``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```PSD2_Portal``` database.

            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2_Portal;Integrated Security=true;Application Name=AltaSoft.PSD2.DataAccess"
            }
            ```

        1. Fill ```Authentication``` section with username and password. This username and password will be used for basic authentication

            ```json
            "Authentication": {
                "UserName": "User",
                "Password": "Password"
            }
            ```

1. ## Install PSD2 DataAccess in IIS

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pool
        
        ![Image](../main/Images/EditAppPool.png)

        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.DataAccess_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field

    1. Create Web Site

         ![Image](../main/Images/AddWebSite.png)

        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.DataAccess``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.DataAccess_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\DataAccess``` into ```Physical path``` field
        1. Configure bindings
        1. Press ```OK```

    1.  That's it. :smiley:
        * Browse this site and ensure, that swagger endpoint is working
        * Check that everything is working as expected
