# Alta Software PSD2 Chargeable Request Admin Installation Guide for IIS

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

    
1. ## Download AltaSoft Chargeable Requests Admin APP 

1. ## Extract AltaSoft Chargeable Requests Admin APP in IIS folders
    
    1. Extract AltaSoftChargeable Requests Admin APP to ```C:\Inetpub\PSD2\AltaSoft.PSD2.ChargeableRequestsAdmin``` folder

    1. Go to folder find ```appsettings.json``` file, open it and:
        1.  Fill ```Psd2Database:ConnectionString``` section with PSD2 database connection string.
            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2;Integrated Security=true;Application Name=AltaSoft.PSD2.InternalApi;Encrypt=false;TrustServerCertificate=true;"
            }
            ```
        1.  If your bank has our PSD2 TPP Solution fill Tpp database connection string.

            Fill ```Psd2TPPDatabase:ConnectionString``` section with PSD2 TPP database connection string.
            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2_TPP;Integrated Security=true;Application Name=AltaSoft.PSD2.InternalApi;Encrypt=false;TrustServerCertificate=true;"
            }
            ```
       
        2.  Fill Options for temporary path. Create  a directory where temporary files will be stored and specify the path in TemporaryDownloadPath (shown below). The folder must have write access set to local system.
        ```json
            "Options": 
            {
             "TemporaryDownloadPath": "D:\\TempDownloads""
            },
         ```
        
        3.  Fill AuthorizationGroups with Active directory group names. Create two groups in Microsoft Active directory for the app. For example, PSD2RequestsAdmin,PSD2RequestsViewer and add desired users to the groups. 
        ```json
        "AuthorizationGroups": {
        "AdminsGroupName": "PSD2RequestsAdmin",
        "ViewersGroupName": "PSD2RequestsViewer"
                }   
        ```        
  
1. ## Install AltaSoft Chargeable Requests Admin APP 

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pools
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.ChargeableRequestsAdmin_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

      

    1. Create Web Site. 

        #### AltaSoft Chargeable Requests Admin APP  **must not be accessible to public** 
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.ChargeableRequestsAdmin``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.ChargeableRequestsAdmin_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\AltaSoft.PSD2.ChargeableRequestsAdmin``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field or **http** can be used as well
        1. if you use https, Select correct SSL Certificate for website
        1. Press ```OK```
        1. Select newly created site, right click it and select ```Edit Bindings...```
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```



1.  That's it. :smiley:
    * Check that everything is working as expected
