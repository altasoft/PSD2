# Consent Request on Account List of Available Accounts
>9.1.4 ყველა გამოსადეგ ანგარიშის ინფორმაციის მოთხოვნა

_ვარიანტი availableAccounts (Mandatory)_
```
"access": {
    "availableAccounts": "allAccounts"
}

ან

"access": {
    "availableAccounts": "allAccountsWithOwnerName"
}
```

**SCA** - არაა აუცილებელი? [ნახეთ რა წერია დოკუმენტაციაში](#References)

_ვარიანტი availableAccountsWithBalance (Optional)_
```
"access": {
    "availableAccountsWithBalance": "allAccounts"
}

ან

"access": {
    "availableAccountsWithBalance": "allAccountsWithOwnerName"
}
```
 
**SCA** - აუცილებელია

---
## განმარტებები

- **"access"** ატრიბუტში რამე სხვა თუ ეწერება, ვაბრუნებთ შეცდომას.

- PSU-სთვის დადასტურების გვერდზე გამოგვაქვს ყველა გამოსადეგი ანგარიში, მაგრამ შეცვლის საშუალებას არ ვაძლევთ. ან ეთანხმება ყველაფერს, ან არა.

- თუ მეორე ვარიანტია, მაშინ ნაშთსაც ვანახებთ დადასტურების გვერდზე.

- ანგარიშის დეტალებზე და ტრანზაქციებზე ამ შემთხვევაში უფლებას არ ვაძლევთ, მხოლოდ სიის წამოღებისას რომ მოხვდეს ეს ანგარიში სიაში

- ნაშთებზე უფლებას ვაძლევთ, თუ მეორე ვარიანტია

---

## კითხვები

- მეორე ვარიანტში, ნაშთებზე თუ უფლებას ვაძლევთ, ნიშნავს ეს, რომ დეტალებზეც ვაძლევთ?

---

## References
SCA-ს შესახებ, Implementation Guide Page 143 წერია ასე:

>_For the first of these specific Consent Requests, no assumptions are made for the SCA Approach by this specification, since there are no balances or transaction information contained and this is then not unambigously required by [EBA-RTS]. It is up to the ASPSP to implement the appropriate requirements on customer authentication._

