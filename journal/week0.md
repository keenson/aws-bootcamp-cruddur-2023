# Week 0 — Billing and Architecture

## Required Homework/Task


### Install AWS CLI
following the AWS documentation, I installed awscli which I then used to perform the budget and billing tasks

```sh
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

![awscli config](assets/budget.png)



### Create an Admin User and Generate AWS Credential
![console IAM user image](assets/IAM-user.png)

![proof of IAM user credential](assets/IAM-cred.png)




### Recreate Conceptual design(Napkin)

![Proof of crudurr conceptual design](assets/conceptual-diagram.jpg)




### Recreate logical design

![Proof of crudurr logical design](assets/crudurr-logical-diagram.png)


[Link to my logical diagram in lucidchart](https://lucid.app/lucidchart/07898a23-5a9e-453f-92cc-66609860e83f/edit?viewport_loc=-485%2C-78%2C3184%2C1620%2C0_0&invitationId=inv_10879534-8d99-41d5-8e0c-9135f8217753)


### Create Budget and Billing Alarm
I created two different budget for my account, one for $1 spend and the other for $2 spend. I also created billing alarm for daily usage

![Image of The Budget I Created](assets/budget2.png) 


![Image of The Budget Alarm I Created](assets/billing-alarm.png) 




---

## Homework Challenges

- I successfully setup MFA and IAM role for on my root account
- I also deleted the credentials on my root account
