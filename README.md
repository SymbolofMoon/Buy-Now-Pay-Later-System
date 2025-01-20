
## Table of Contents
- [Buy Now, Pay Later System](#BuyNowPayLaterSystem)
- [Functional Requirements](#functional-requirements) 
- [NonFunctional Requirements](#nonfunctional-requirements)
- [Bonus Points](#Bonus-Points)
- [Database Schema](#database-schema)
- [API Endpoints](#api-endpoints)
- [Points to consider](#points-to-consider)
- [High Level System Design](#high-level-system-design)
- [Low Level System Design](#low-level-system-design)
- [Bonus Point Discussion](#bonus-points-discussion)
# Buy Now, Pay Later System

  We want to design a system where users can purchase items on credit and repay the borrowed amount using fixed monthly payments or EMIs. The system should calculate penalties for late payments and interest for EMIs.

## Functional Requirements
  - User Registration & Credit Assignment:
      - Users have a maximum credit limit.
      - Users can check their available credit.
  - Purchases:
      - Users can make purchases if they have sufficient available credit.
      - Deduct purchase amounts from the available credit.
  - Repayment Plans:
      - Users can choose between:
          -  Fixed Payment for a particular purchase.
          -  EMI Plan: Monthly installments with interest over a fixed number of months.
      - Calculate and store the repayment schedule in memory.
      - Add repayment amount to available credit.
  - Penalties for Late Payments:
      - Calculate penalties for overdue payments based on country-specific rates.
  - Reports & Queries:
      - Fetch outstanding balance, repayment schedules, and penalties.


## NonFunctional Requirements


## Bonus Points
-  Secure API via oAuth2
-  Optimize API for 100K RPM
-  Establish CI/CD Pipeline (feel free to use open-source tools)
-  Create a messaging event that publishes the username and the image name to a Messaging Platform (Ex: Kafka)
    & feel free to connect with a local or cloud instance of the Messaging Platform.
-  Preferably following the TDD approach for Junit test cases

## Database Schema
-  We are using H2(In Memory Database) as given in the requirement
-  There are two relational tables which are as follows:
    -  User
        -  id: Generated ID
        -  username: Unique Parameter
        -  password: Storing password in encrypted form with the help of Bcrypt
        -  email: Unique Parameter
        -  List<Image>images: List of images and OnetoMany relations
    -  Image
        -  id: Generated ID
        -  imgurId: Id that ImgurAPI generates
        -  imgurLink: The Image link that is uploaded to Imgur
        -  imgurDeleteHash: The delete hash used to delete the image in Imgur
        -  imgurName: The name of the file that is uploaded

## API Endpoints
  - 
    - POST /api/users/register: Registers a user with username and password and basic information.
    - GET /api/users/credit:  Check the available credit
    - POST /api/users/login: Authenticates user based on username and password
    - POST /api/users/logout: Logout the current user
    - GET /api/users/plans: This would get us all the active EMIs and the total outstanding balance.
    - GET /api/users/plans/{emiid}: Get EMI wise outstanding balance as well
    - POST /api/purchase: Purchase item, user can select purchasing type and save to database, update the balance in the credit of the user
        - Request: {
                      "user_id": "U12345",
                        "amount": 20000,
                        "type": "EMI",
                        "emi_months": 6
          }
    - POST /api/purchase/plan: While Purchasing using will create a  repayment plan and it will be save to database
    - POST /payments: User will pay the amount to Repayment Service and it will check penalties for overdue payments based on country-specific rate. Later it will save the repayment amount to credit balance of the user
    - GET /reports?date_range=()& user_ids=()&amount range=(): Get all outstanding payments which can be filtered by date range, user_ids, amount range
    - GET /history/user_id?={user_id}: Get repayment history of a user


## High level system design
-  This is the Flow
  ![Logo](images.png)


