# Alta Software PSD2 Solution Installation Guide for IIS

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more detailed instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## Run AltaSoft.PSD2.Iis.Installer.exe ***as administrator***

    1.  After program is loaded, you will see the login screen:

        ![Image](../main/Images/Installer-login.png)

        * Fill Bank BIC code - Your bank's 9 digit SWIFT BIC code
        * Type your password and then press 'Next' button

    1.  Now you will see the Database and Certificate screen:

        ![Image](../main/Images/Installer-sql.png)

        * Fill SQL connection string *(You can use Sql connection string dialog by pressing '...' button)*
        * Fill your QSealC certificates thumbprint and press 'Next' button *(You can use Select Certificate dialog by pressing '...' button)*
            * Your QSealC certificate should be installed in ```Windows Certificate Store``` with store location = ```Local Machine```. 
            * It should have private key.

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

    1.  That's it. :smiley:
        * Check that everything is working as expected
