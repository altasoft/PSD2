# B7.Orbit სერვისი

1. ## წინაპირობები ##

    1. Windows Server 2019/2022 ან Windows 10/11

    1. SQL Server 2019+

    1. .NET Runtime (ASP.NET Core Hosting Bundle) v9.0+ – დააყენეთ სერვერზე (IIS ჰოსტინგისთვის სავალდებულოა)

    1. უფლება: Command Prompt Run as Administrator

    1. გამართული B7.Pushing სერვისი

1. ## B7.Orbit მონაცემთა ბაზის შექნმა
    1. გაუშვით SQL Server Management Studio (SSMS).
    1. დაუკავშირდით SQL Server-ს.
    1. გახსენით ახალი Query ფანჯარა და გაუშვით:
        ```sql
        CREATE DATABASE [Orbit] COLLATE Latin1_General_100_BIN2;
        ```

1. ## გაამზადეთ B7.Orbit
    1. გადმოწერეთ B7.Orbit-ის ფაილები
        1. Migration.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/Migration.zip)
        1. Api.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/Api.zip)
        1. Sync.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/Sync.zip)
        1. SyncB6.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/SyncB6.zip)
        1. SyncPCard.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/SyncPCard.zip)
        1. SyncGlobalTransfers.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/SyncGlobalTransfers.zip)
        1. SyncImpExp.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/SyncImpExp.zip)
        1. SyncAccountNotifications.zip [Direct download](https://customer-portal.altasoft.ge/fileManagement?path=Home/ISO20022/Orbit/SyncAccountNotifications.zip)
        
    1. გახსენით ```Migration.zip``` ფაილი ```\B7.Orbit\Migration``` ფოლდერში
    1. გახსენით ```Api.zip``` ფაილი ```\B7.Orbit\Api``` ფოლდერში
    1. გახსენით ```Sync.zip``` ფაილი ```\B7.Orbit\Sync``` ფოლდერში
    1. გახსენით ```SyncB6.zip``` ფაილი ```\B7.Orbit\SyncB6``` ფოლდერში
    1. გახსენით ```SyncPCard.zip``` ფაილი ```\B7.Orbit\SyncPCard``` ფოლდერში
    1. გახსენით ```SyncGlobalTransfers.zip``` ფაილი ```\B7.Orbit\SyncGlobalTransfers``` ფოლდერში
    1. გახსენით ```SyncImpExp.zip``` ფაილი ```\B7.Orbit\SyncImpExp``` ფოლდერში
    1. გახსენით ```SyncAccountNotifications.zip``` ფაილი ```\B7.Orbit\SyncAccountNotifications``` ფოლდერში

    1. შედით ```Migration``` ფოლდერში, გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და:

        1.  ```Settings``` სექციაში ჩაწერეთ სასურველი მონაცემები

            თუ არ იყენებთ ჩვენ PCards სისტემას, მაშინ ``cards`` სექცია ამოშალეთ.

            თუ არ იყენებთ ჩვენ LMS სისტემას, მაშინ ``Loans`` სექცია ამოშალეთ.
            
            ```json
            "Settings": {
                "StartYear": 2020, // ეს არის წელი, რომლიდანაც დაიწყება მონაცემების მოიგრაცია.

                "BatchSize": 10000,
                "ParallelProcessCount": 4,

                "Databases": {
                    // BANK2000 ბაზის ConnectionString
                    "B6": "Server=%SQL_SERVER_B6%;Database=BANK2000;Integrated Security=true;Application Name={ApplicationName};Encrypt=false;TrustServerCertificate=true;",
                    // CARDS2011 ბაზის ConnectionString
                    "Cards": "Server=%SQL_SERVER_CARDS%;Database=CARDS2011;Integrated Security=true;Application Name={ApplicationName};TrustServerCertificate=true;TrustServerCertificate=true",
                    // LMS ბაზის ConnectionString
                    "Loans": "Server=%SQL_SERVER_LOANS%;Database=LMS;Integrated Security=true;Application Name={ApplicationName};TrustServerCertificate=true;TrustServerCertificate=true"
                }
            },
            ```

            * StartYear განსაზღვრავს, რომელი წლიდან დაიწყოს მონაცემების მიგრაცია.

        1.  ```Database``` სექციაში ჩაწერეთ ```Orbit``` მონაცემთა ბაზის ```ConnectionString```
            ```json
            "Database": {
                "ConnectionString": "Server=%SQL_SERVER%;Database=Orbit;Integrated Security=true;Application Name={ApplicationName};TrustServerCertificate=true;",
            }

        1.  ```AppEnvironment``` სექციაში ჩაწერეთ ```Testing``` ან ```Production```
            ```json
            "AppEnvironment": {
                "Type": "Testing"
            }
            ```
        
    1. შედით ```Api``` ფოლდერში, გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და ```Authentication``` სექციაში ჩაწერეთ სასურველი ```username``` და ```password```. ეს პარამეტრები გამოიყენება basic authentication-ისათვის.
        ```json
        "Authentication": {
            "UserName": "ChangeThisUser",
            "Password": "ChangeThisStrongPassword"
        }
        ```
1. ## დაამიგრირეთ არქივული ტრანზაქციები
    
    1. გახსენით Command Prompt (Admin).
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება
        ```
            _migrate_transaction_arc.bat
        ```
        > ### ⚠️ მნიშვნელოვანია
        > პროცესის დამთავრებას დასჭირდება ძალიან დიდი ხანი.  
        > გთხოვთ ლოგი გამოგვიგზავნოთ პერფორმანსის გასაანალიზებლად.

        > ### ⚠️ ძალიან მნიშვნელოვანია
        > გახსენით მიგრაციის ლოგი და ნახეთ ხომ არ არის რამე შეცდომა (ERR).
        > ასევე ნახეთ ხომ არ არის გაფრთხილება (WARN).
        > მიგრაციის შემდგომ, ავტომატურად დარდება ანგარიშის ნაშთები B7.Orbit ბაზასა და BANK2000 ბაზას შორის. ნემისმიერი განსხვავება დაილოგება WARN სტატუსით.

1. ## დაამიგრირეთ დანარჩენი მონაცემები 
    1. დარწმუნდით, რომ წინა პროცესი დასრულდა
    1. შედით ```Migration``` ფოლდერში 
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება
        ```
            _migrate_final.bat 
        ```

        > ### ⚠️ მნიშვნელოვანია
        > პროცესის დამთავრებას დასჭირდება დიდი ხანი.  
        > გთხოვთ ლოგი გამოგვიგზავნოთ პერფორმანსის გასაანალიზებლად.

        > ### ⚠️ ძალიან მნიშვნელოვანია
        > გახსენით მიგრაციის ლოგი და ნახეთ ხომ არ არის რამე შეცდომა (ERR).
        > ასევე ნახეთ ხომ არ არის გაფრთხილება (WARN).
        > მიგრაციის შემდგომ, ავტომატურად დარდება ანგარიშის ნაშთები B7.Orbit ბაზასა და BANK2000 ბაზას შორის. ნემისმიერი განსხვავება დაილოგება WARN სტატუსით.

1. ## გაუშვით B7.Orbit.Api
    1. დააინსტალირეთ ASP.NET Core Hosting Bundle (v9.0+) სერვერზე.
    1. განათავსეთ \Orbit\Api როგორც IIS აპლიკაცია.
    1. დააყენეთ Application Pool: No Managed Code, 64-ბიტი ჩართული.
    1. შეამოწმეთ, რომ ჩართული აქვს read/write უფლებები საკუთარ Log ფოლდერზე.

1. ## სერვისების ინსტალაცია – SyncB6 (და სხვა Sync სერვისები)
    ყველა ბრძანება გაუშვით Admin CMD-დან და მიუთითეთ აბსოლუტური ბილიკები. რეკომენდირებულია ავტომატური გაშვება.

    მაგალითი – SyncB6

    1. შედით ``SyncB6`` ფოლდერში 
    1. გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და შეავსეთ სასურველი მონაცემებით
    1. Windows-ის command line-იდან გაუშვით შემდეგი ბრძანება:
        ```
            sc create B7.Orbit.SyncB6 binPath= "C:\Orbit\SyncB6\B7.Orbit.SyncB6.exe" start= auto
            sc start B7.Orbit.SyncB6
        ```

1. ## გაუშვით დანარჩენი სერვისები
    1. შედით თითოეულ ``Sync*`` ფოლდერში
    1. გახსენით ```appsettings.json``` ფაილი ნებისმიერი ტექსტური რედაქტორით და შეავსეთ სასურველი მონაცემებით
    
    შემდეგ სათითაოდ გაუშვით ყველა სერვისი

    ```
    sc create B7.Orbit.Sync binPath= "C:\Orbit\Sync\B7.Orbit.Sync.exe" start= auto
    sc create B7.Orbit.SyncPCard binPath= "C:\Orbit\SyncPCard\B7.Orbit.SyncPCard.exe" start= auto
    sc create B7.Orbit.SyncGlobalTransfers binPath= "C:\Orbit\SyncGlobalTransfers\B7.Orbit.SyncGlobalTransfers.exe" start= auto
    sc create B7.Orbit.SyncImpExp binPath= "C:\Orbit\SyncImpExp\B7.Orbit.SyncImpExp.exe" start= auto
    sc create B7.Orbit.SyncAccountNotifications binPath= "C:\Orbit\SyncAccountNotifications\B7.Orbit.SyncAccountNotifications.exe" start= auto

    sc start B7.Orbit.Sync
    sc start B7.Orbit.SyncPCard
    sc start B7.Orbit.SyncGlobalTransfers
    sc start B7.Orbit.SyncImpExp
    sc start B7.Orbit.SyncAccountNotifications
    ```
1. ## შეამოწმეთ, რომ ყველაფერი მუშაობს

1. ## შემოწმება
    1. IIS-ში გახსენით B7.Orbit.Api და გადაამოწმეთ, რომ Web სერვისი მუშაობს შეცდომების გარეშე (მაგ. `/health` endpoint-ზე).
    1. გადაამოწმეთ Windows სერვისების სტატუსი:
        ```
        sc query B7.Orbit.SyncB6
        sc query B7.Orbit.Sync
        sc query B7.Orbit.SyncPCard
        sc query B7.Orbit.SyncGlobalTransfers
        sc query B7.Orbit.SyncImpExp
        sc query B7.Orbit.SyncAccountNotifications
        ```
    1. დარწმუნდით, რომ ყველა სერვისი გაშვებულია და სტატუსი არის `RUNNING`.
    1. გადაამოწმეთ ლოგ ფაილები (`Logs` საქაღალდეებში), რომ არ არის კრიტიკული შეცდომები.
    1. შეამოწმეთ, რომ Orbit მონაცემები სინქრონიზდება და API პასუხობს მოთხოვნებზე.
