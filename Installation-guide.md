# Alta Software PSD2 Solution Installation Guide for IIS

1. ## Install IIS 10

1. ## Install the ASP.NET Core Module/Hosting Bundle

    Download the installer using the following link:
    [Current .NET Core Hosting Bundle installer direct download](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

    For more details instructions on how to install the ASP.NET Core Module see [Install the .NET Core Hosting Bundle.](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/hosting-bundle?view=aspnetcore-5.0)

1. ## Run AltaSoft.PSD2.Iis.Installer.exe ***as administrator***

    1.  After program is loaded, you will see this screen:

        ![Image](../main/Images/Installer-settings.png)

  
        * Fill all settings
            * Bank BIC code - Your bank's 9 digit SWIFT BIC code
            * Base hostname - Base hostname for PSD2 Web services/sites. Should match your SSL certificate
            * Base location - Base physical path, where all PSD2 related Web services/sites will be installed
            * Instance Id - Id of this instance of IIS. Should be positive integer number and unique across your infrastructure

        * Enter password and then press 'Authenticate' button

    1.  Now you will see this screen:

         ![Image](../main/Images/Installer-sandbox.png)

        * Verify that Location & Hostname-s are correct
        * Press 'Download' button
        * Wait while new version is being downloaded
        * Press 'Install' button
        * You will be prompted to select SSL certificate for this site. 
        * Please choose the right one and press 'OK'


    1.  That's it. 
        * Check that everything is working as expected
