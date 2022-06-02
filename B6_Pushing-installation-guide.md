# B6-ის ობიექტების ცვლილების გამოქვეყნების სერვისი (B6.Pushing.Lite)

1. ## დააყენეთ RabbitMQ [Direct download](https://www.rabbitmq.com/download.html)
1. ## გაამზადეთ B6.Pushing.Lite
    
    1. გახსენით ```B6.Pushing.zip``` ფაილი ```\B6.Pushing``` ფოლდერში
    
    1. გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და:

        1.  ```DbProviders``` სექციაში ჩაწერეთ ```B6``` და ```PCards``` ელემენტებში მონაცემთა ბაზის პარამეტრები
            ```json
            "DbProviders": {
                "B6": "Data Source=Sql;Initial Catalog=BANK2000;Integrated Security=true;Application Name=B6.Pushing;Encrypt=false;TrustServerCertificate=true",
                "PCards": "Data Source=Sql;Initial Catalog=CARDS2011;Integrated Security=true;Application Name=B6.Pushing;Encrypt=false;TrustServerCertificate=true"
            }
            ```
        
        2. ```BusProviders``` სექციაში ჩაწერეთ ```B6.Events```,  ```B6.Transactions``` ელემენტებში ```RabbitMq``` პარამეტრები
            ```json
            "BusProviders": {
                "B6.Events": {
                    "ConnectionString": "HostName=RabbitMq;VirtualHost=B6;UserName=username;Password=password;ClientProvidedName=B6.Pushing.Events;Encrypt=false;TrustServerCertificate=true",
                    "TopicName": "B6.Events"
                },
                "B6.Transactions": {
                    "ConnectionString": "HostName=RabbitMq;VirtualHost=B6;UserName=username;Password=password;ClientProvidedName=B6.Pushing.Transactions;Encrypt=false;TrustServerCertificate=true",
                    "TopicName": "B6.Transactions"
                }
            }
            ```
        
        3. ```AppEnvironment``` სექციაში ჩაწერეთ ```Testing``` ან ```Production```
            ```json
            "AppEnvironment": {
                "Type": "Testing"
            }
            ```

        4. გახსენით  ```hosting.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და გაუწერეთ თავისუფალი პორტი (შეგიძლიათ არ შეცვალოთ და დატოვოთ რაც არის):
            ```json
            {
                "urls": "http://*:9012"
            }
            ```
        
1. ## დააყენეთ b6.pushing Windows Service-ად და გაუშვით
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება:
        ```
            sc create B6.Pushing binPath= "C:\B6.Pushing\B6.Pushing.PSD2.exe"
            sc start B6.Pushing
        ```
