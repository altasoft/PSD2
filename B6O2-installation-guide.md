# B6-ის AlwaysOn სერვისი (B6O2 Lite)

1. ## წინასწარ დაყენებული უნდა გქონდეთ B6.Pushing სერვისი

1. ## B6O2 მონაცემთა ბაზის შექნმა
    1. Run SQL Server Management Studio
    1. Connect to SQL Server
    1. Open new query window
    1. Create database with the following command:
        ```sql
        CREATE DATABASE [B6O2] COLLATE Latin1_General_100_BIN2;
        ```

1. ## გაამზადეთ B6O2
    1. გადმოწერეთ b6o2-ის ფაილები
        1. B6O2_Migration.zip [Direct download](https://psd2files.altasoft.ge/B6O2.Lite/B6O2_Migration.zip)
        1. B602_EventHandler.zip [Direct download](https://psd2files.altasoft.ge/B6O2.Lite/B6O2_EventHandler.zip)
        1. B6O2_Api.zip [Direct download](https://psd2files.altasoft.ge/B6O2.Lite/B6O2_Api.zip)

    1. გახსენით ```B6O2_Migration.zip``` ფაილი ```\B6O2\Migration``` ფოლდერში
    1. გახსენით ```B6O2_EventHandler.zip``` ფაილი ```\B6O2\EventHandler``` ფოლდერში
    1. გახსენით ```B6O2_Api.zip``` ფაილი ```\B6O2\Api``` ფოლდერში
    1. შედით თითოეულ ფოლდერში, გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და:

        1.  ```Database``` სექციაში ჩაწერეთ ```B6O2``` მონაცემთა ბაზის ```ConnectionString```
            ```json
            "Database": {
                "ConnectionString": "Data Source=localhost;Initial Catalog=B6O2;Integrated Security=true;Application Name=B6O2;Encrypt=false;TrustServerCertificate=true"
            }
            ```

        2.  ```QueueProviders\Default``` სექციაში ჩაწერეთ ```RabbitMQ``` პარამეტრები
            ```json
            "QueueProviders": {
                "Default": {
                    "ConnectionString": "HostName=RabbitMq;VirtualHost=B6;UserName=username;Password=password;ClientProvidedName=B6O2;Encrypt=false;TrustServerCertificate=true"
                }
            }
            ```

        3.  ```AppEnvironment``` სექციაში ჩაწერეთ ```Testing``` ან ```Production```
            ```json
            "AppEnvironment": {
                "Type": "Testing"
            }
            ```
        
    1. შედით ```Api``` და ```EventHandler``` ფოლდერში, გახსენით  გახსენით ```hosting.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და გაუწერეთ თავისუფალი პორტი (შეგიძლიათ არ შეცვალოთ და დატოვოთ რაც არის):
        ```json
        {
            "urls": "http://*:23001"
        }
        ```

    1. შედით ```Api``` ფოლდერში, გახსენით  გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და ```Authentication``` სექციაში ჩაწერეთ ```username``` და ```password```. ეს პარამეტრები გამოიყენება basic authentication-ისათვის.
        ```json
        "Authentication": {
            "UserName": "User",
            "Password": "Password"
        }
        ```
1. ## დაამიგრირეთ არქივული ტრანზაქციები
    1. შედით ```Migration``` ფოლდერში, გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და:
        1. ```Settings``` სექციაში ჩაწერეთ ```StartYear``` პარამეტრი. ეს არის წელი, რომლიდანაც დაიწყება მონაცემების მოიგრაცია.
            ```json
            "Settings": {
                "StartYear": 2020
            }
            ```
        1. ```Settings``` სექციაში ჩაწერეთ ```B6ConnectionString``` პარამეტრი. ეს არის B6 მონაცემთა ბაზის ConnectionString. საჭიროა, რომ მიგრაციამ მაქედან გადმოიტანოს მონაცემები.
            ```json
            "Settings": {
                "B6ConnectionString": "Data Source=SQL;Initial Catalog=BANK2000;User Id=b2000;Password=1234;Max Pool Size=200;Application Name=AltaSoft.B6O2.Migration;Encrypt=false;TrustServerCertificate=true"
            }
            ```
        1. ```Settings``` სექციაში ჩაწერეთ ```CardsConnectionString``` პარამეტრი. ეს არის CARDS2011 მონაცემთა ბაზის ConnectionString. საჭიროა, რომ მიგრაციამ მაქედან გადმოიტანოს მონაცემები. თუ არ გაქვთ ეს პროდუქტი, უბრალოდ ამოშალეთ ეს ჩანაწერი.
            ```json
            "Settings": {
                "CardsConnectionString": "Data Source=SQL;Initial Catalog=BANK2000;User Id=b2000;Password=1234;Max Pool Size=200;Application Name=AltaSoft.B6O2.Migration;Encrypt=false;TrustServerCertificate=true"
            }
            ```
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება
        ```
            _migrate_transaction_arc.bat 
        ```
    1. პროცესის დამთავრებას დასჭირდება ძალიან დიდი ხანი

1. ## დაამიგრირეთ დანარჩენი მონაცემები 
    1. დარწმუნდით, რომ წინა პროცესი დასრულდა
    1. შედით ```Migration``` ფოლდერში 
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება
        ```
            _migrate_final.bat 
        ```
    
1. ## გაუშვით B6O2.EventHandler
    1. შედით ```EventHandler``` ფოლდერში 
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება:
        ```
            sc create B6O2.EventHandler binPath= "\B6O2\EventHandler\AltaSoft.B6O2.EventHandler.exe"
            sc start B6O2.EventHandler
        ```

1. ## გაუშვით B6O2.Api
    1. დაჰოსტეთ B6O2.Api IIS 10-ზე

1. ## შეამოწმეთ, რომ ყველაფერი მუშაობს
