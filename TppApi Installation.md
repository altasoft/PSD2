# Alta Software PSD2 TPP API  Installation Guide for IIS

1. ## Install IIS 10

1. ## Delete all non root certificates from ```Trusted root certificates```
    You should delete any certificate whose ```Issued by``` and ```Issued to``` values are not the same (and therefore the certificate is not at the top of the hierarchy).
    
    Plese see this: https://docs.microsoft.com/en-us/troubleshoot/iis/http-403-forbidden-open-webpage


1. ## Install your QWAC and Qsealc certificates in ```Personal``` with store location = ```Local Machine```. 


1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)


    
1. ## (Conditional) Install SQL Server Express (2016+) or you can use existing SQL server (Enterprise)
    
    1. Download the installer using the following link:
        [Latest SQL Server Express installer direct download](https://go.microsoft.com/fwlink/?linkid=866658)
    
        If you already have SQL server installed somewhere, you can use it.

    1. Run SQL Server Management Studio
    1. Connect to SQL Server
    1. Open new query window
    1. Create database with the following command:

    ```sql
    CREATE DATABASE [PSD2_TPP] COLLATE Latin1_General_100_BIN2;
    ```

1. ## Download PSD2 TPP API & TPP API Redirect 

1. ## Extract PSD2 TPP API  in IIS folders
    
    1. Extract PSD2 TPP Api to ```C:\Inetpub\PSD2\TppApi``` folder

    1. Go to folder find ```appsettings.json``` file, open it and:
        1.  Fill ```Database:ConnectionString``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```PSD2_TPP``` database. 
            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2_Portal;Integrated Security=true;Application Name=AltaSoft.PSD2"
            }
            ```

        1. Fill ```RedirectApiBaseAddress``` with Tpp Api Redirect Url 

        1. Fill ```"CertificateThumbprint``` with your Qwac and  QSealC certificate's thumbprint
            ```json
                 "Qwac": "f877665bf66ff3e5fe5810c37c7280543af9bec8",
                 "QSealC": "eb80f741219f38ccc1e18519be7a789efe5328db"

            ```
   
        2. Fill ```ValidateServerQwac``` with true for production environment (checks if xs2a has correct Qwac certificate as ssl)
            
        3. ```AisProviderSettings ``` is the urls for ASPSP adjust as required   
  

1. ## Extract PSD2 TPP API REDIRECT  in IIS folders
    
    1. Extract PSD2 TPP Api Redirect to ```C:\Inetpub\PSD2\TppApiRedirect``` folder

    1. Go to folder find ```appsettings.json``` file, open it and: **must be same as TPP API**
        1.  Fill ```Database:ConnectionString``` section with database connection string. Database user ***must*** have ```db_owner``` rights on ```PSD2_TPP``` database. 
            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=PSD2_Portal;Integrated Security=true;Application Name=AltaSoft.PSD2"
            }
            ```

       

        1. Fill ```"CertificateThumbprint``` with your Qwac and  QSealC certificate's thumbprint **must be same as TPP API**
            ```json
                 "Qwac": "f877665bf66ff3e5fe5810c37c7280543af9bec8",
                 "QSealC": "eb80f741219f38ccc1e18519be7a789efe5328db"

            ```
        1. Fill ```"IBankOptions``` with your Internet bank Not Ok url (where we will send you that an error occured)
            ```json
                 "NokRedirectUri": "f877665bf66ff3e5fe5810c37c7280543af9bec8",
            ```
   
        2. Fill ```ValidateServerQwac``` with true for production environment (checks if xs2a has correct Qwac certificate as ssl)
            
        3. ```AisProviderSettings ``` is the urls for ASPSP adjust as required   
  

1. ## Install PSD2 TPP API & TPP API REDIRECT in IIS

    1. Go to Internet Information Services (IIS) Manager
    1. Create Application Pools
        

        #### TPP API
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.TppApi_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

        #### TPP API Redirect
        1. Select ```Application Pools```, right click it and select ```Add Application Pool...```
        1. Enter ```AltaSoft.PSD2.TppApiRedirect_AppPool``` into ```name``` field
        1. Select ```No Managed Code``` in ```.NET CLR version``` field
        1. Select ```Integrated``` in ```Managed pipeline mode``` field
        1. Press ```OK```
        1. Select newly created application pool, right click it and select ```Advanced Settings...```
        1. Set ```General\Start mode``` to ```AlwaysRunning```
        1. Set ```Process Model\Identity``` to ```LocalSystem```
        1. Set ```Process Model\Idle Time-out (minutes)``` to ```0```
        1. Set ```Recycling\Disable Overlapped Recycle``` to ```False```

    

    1. Create Web Sites. If you want you can install these 2 APIS on different machines


        #### TPP API This must be an Internal APi and **must not be accessible to public**
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.TppApi``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.TppApi_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\TppApi``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Select correct SSL Certificate for website
        1. Press ```OK```
        1. Select newly created site, right click it and select ```Edit Bindings...```
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```

        ####  TPP API Redirect. **This must be Public**
        1. Select ```Sites```, right click it and select ```Add Website...```
        1. Enter ```AltaSoft.PSD2.TppApiRedirect``` into ```Site name``` field
        1. Select ```AltaSoft.PSD2.TppApiRedirect_AppPool``` in ```Application pool``` field
        1. Enter ```C:\Inetpub\PSD2\TPP\TppApiRedirect``` into ```Physical path``` field
        1. Select ```https``` in ```Binding: Type``` field
        1. Enter ```yourdomain.yourbank.ge```  in ```Binding: Host name``` field and select ```Require Server Name Indication``` - url should match one specified in QWAC certificate
        1. Select ```*.yourdomain.ge``` certificate in ```Binding: SSL certificate``` field
        1. Press ```OK```
        1. Press ```Apply``` button
        1. Right click the site it and select ```Manage website\Advanced Settings...```
        1. Set ```General\Preload Enabled``` to  ```True```



1.  That's it. :smiley:
    * Check that everything is working as expected