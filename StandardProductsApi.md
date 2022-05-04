# Alta Software PSD2 StandardProducts API Installation Guide for IIS

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

    
1. ## Download AltaSoft StandardProducts API 

1. ## Extract AltaSoft StandardProducts API in IIS folders
    
    1. Extract AltaSoft StandardProducts API to ```C:\Inetpub\PSD2\StandardProducts``` folder

    1. Go to folder find ```appsettings.json``` file, open it and:
        1.  If needed Modify ProductData section
        ```json 
        "ProductData": {
          "PathToJsonEn": "D:\\PSD2\\StandardProducts\\ExampleData.json",
          "PathToJsonKa": "D:\\PSD2\\StandardProducts\\ExampleData.json",
          "PageSize": 25
        }
        ```
  
1. ## Install PSD2 StandardProducts API

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pools
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.StandardProducts_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

      

    1. Create Web Site. 

        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.StandardProducts``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.StandardProducts_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\StandardProducts``` into ```Physical path``` field
        1. Select correct SSL Certificate for website
        1. Press ```OK```
        1. Select newly created site, right click it and select ```Edit Bindings...```
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```



1.  That's it. :smiley:
    * Check that everything is working as expected
