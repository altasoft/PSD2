# Alta Software PSD2 Solution Installation Guide for IIS

1. ## Install IIS 10

1. ## Delete all non root certificates from ```Trusted root certificates```
    You should delete any certificate whose ```Issued by``` and ```Issued to``` values are not the same (and therefore the certificate is not at the top of the hierarchy).
    
    Plese see this: https://docs.microsoft.com/en-us/troubleshoot/iis/http-403-forbidden-open-webpage

1. ## Disable weak protocols, cipher suites and hashing algorithms
    Please read this: 
        [Transport Layer Security (TLS) registry settings](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings)

    and this
        [Manage Transport Layer Security (TLS)](https://docs.microsoft.com/en-us/windows-server/security/tls/manage-tls)

1. ## Install your SSL certificate in ```Personal``` with store location = ```Local Machine```. 

1. ## If you have own QSealC compatible private certificate
    Install it in ```Personal``` with store location = ```Local Machine```. It MUST have private key.

1. ## If you do not have own QSealC compatible private certificate
 
    1. Install AltaSoft PSD2 root certificate in ```Trusted root certificate authorities``` with store location = ```Local Machine```. 
        You can download from here: [AltaSoft PSD2 certificate direct download](https://psd2files.altasoft.ge/altasoftPsd2RootCa.zip). Password is: ```123456```.
 
    2. Install AltaSoft PSD2 Test QCealC  certificate in ```Personal``` with store location = ```Local Machine```. 
        You can download from here: [AltaSoft PSD2 certificate direct download](https://psd2files.altasoft.ge/alta_aspsp_QsealC.zip). Password is: ```123456```.

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## Download PSD2_Sandbox_Installer
    Download the installer using the following link: 
    [Current PSD2 Sandbox Installer direct download](https://psd2files.altasoft.ge/PSD2_Sandbox_Installer.zip)
    
1. ## Run AltaSoft.PSD2.Iis.Installer.exe as ```administrator```

    1.  After program is loaded, you will see the login screen:

        ![Image](../main/Images/Installer-login.png)

        * Fill Bank BIC code - Your bank's 9 digit SWIFT BIC code
        * Select Sandbox or Real
        * Type your password (1234) and then press 'Next' button

    1.  Now you will see the Database and Certificate screen:

        ![Image](../main/Images/Installer-sql.png)

        * Select ```SQL Server``` or ```DataAccess API```
        * If you selected ```SQL Server```, then fill SQL connection string for SQL server, where PSD2 database will be created. Database user ***must*** have ```db_owner``` rights on ```PSD2_Portal``` database *(You can use Sql connection string dialog by pressing '...' button)*
        * If you selected ```DataAccess API```, then fill DataAccess API-s URL and aslo Username and password for basic authentication
        * Fill your QSealC certificates thumbprint and press 'Next' button *(You can use Select Certificate dialog by pressing '...' button)*

        ![Image](../main/Images/Installer-sql-dialog.png)

    1.  Now you will see the Settings  screen:

        ![Image](../main/Images/Installer-settings.png)

        * Fill all settings
            
            * Base hostname - Base hostname for PSD2 Web services/sites. Should match your SSL certificate
            * Base location - Base physical path, where all PSD2 related Web services/sites will be installed
            * Load balancer - Check this if you have load balancer in front of IIS
       
        * Verify that all settings are good and then press 'Next' button

    1.  Now you will see Dev.Portal & Sandbox API screen:

         ![Image](../main/Images/Installer-sandbox.png)

        * Verify that Location & Hostname-s are correct
        * Press 'Download' button
        * Wait while new version is being downloaded
        * Press 'Install' button
        * You will be prompted to select SSL certificate for this site. 
        * Please choose the right one and press 'OK'

        ![Image](../main/Images/Installer-ssl-cert.png)

<!-- 1.  ## !!! Temporary, until real certificates will be available !!!
    Go to each folder (except authweb-sandbox), find ```appsettings.json``` file, open it and and fill ```RootCertificateThumbprints``` parameter with thumbprints of all PSD2 root certificates.
        
    Include your root PSD2 certificate (Generated according to [Banking association of Georgia document](https://bitbucket.org/obgeo/development-ca/src/master/))
        
    Please also include temporarily Alta Sofwate PSD2 root certificate thumprint ```F9AA2ABAD10F448F7F3759AA4B3798ED19A0CCDD``` for testing purposes.
        
    Separate thumprints with ```,``` (comma) character

    ```json
    "RootCertificateThumbprints": "e06aed78542ee8a994c229489c6e31909e7e6441,F9AA2ABAD10F448F7F3759AA4B3798ED19A0CCDD"
    ```
 -->

1.  That's it. :smiley:
    * Check that everything is working as expected
