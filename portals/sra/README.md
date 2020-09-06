# Smile REST API

- [Smile REST API](#smile-rest-api)
    - [Registration](#registration)
    - [Login](#login)
    - [Get Customer Details](#get-customer-details)
    - [Get Customer Without Token](#get-customer-without-token)
    - [Change Password](#change-password)
    - [Forget Password](#forget-password)
    - [Get Accounts](#get-accounts)
    - [Get Account for smile number](#get-account-for-smile-number)
    - [Get Bundles](#get-bundles)
    - [Redeem Voucher](#redeem-voucher)
    - [Sale Airtime Using Payment Gateway](#sale-airtime-using-payment-gateway)
    - [Sale Bundle Using Payment Gateway](#sale-bundle-using-payment-gateway)
    - [Get Sale Status](#get-sale-status)
    - [Sale Bundle Using Airtime](#sale-bundle-using-airtime)
    - [Share Airtime](#share-airtime)
    - [Share Data](#share-data)


## Registration
  Register a new user to the application using OTP.

* **URL**

  /sra/registerbyotp

* **Method:**

  `POST`
    
*  **URL Params**

   None

* **Query Params**

  None
  
* **Request Body:**

  ```
  {
    "customerId":12345,
    "identity":"09876485982", //mobile number used for OTP
    "key":"594432", // OTP Code. Valid for 15 Min
    "newPassword":"test123",
    "confirmPassword":"test123"
  }
  ```

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
      "done" : true
      }
      ```
 
* **Error Response:**

  * **Code:** 400 <br />
    **Content:** 
      ```
      {
      "SRAError":{
      "errorDesc":"identity is missing",
      "errorType":"business",
      "errorCode":"SRA-0003"
      }
      }
      ```
      
  * **Code:** 404 <br />
    **Content:** 
      ```
      {
      "SRAError":{
      "errorDesc":"invalid identity",
      "errorType":"business",
      "errorCode":"SRA-0003"
      }
      }
      ```
      
  * **Code:** 404 <br />
    **Content:** 
      ```
      {
      "SRAError":{
      "errorDesc":"user already regisstered",
      "errorType":"business",
      "errorCode":"SRA-0003"
      }
      }
      ```
   
## Login

User must need to have valid token to access Smile portal APIs. provide a vaid username or smile number with your password to receive a token. 

* **URL**

  /sra/tokens

* **Method:**

  `GET`
    
*  **URL Params**

   None

* **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | username              | Y        | user name                           |
  | password              | Y        | password                            |
  | srav                  | Y        | API version(eg. 1.0, 2.0            |
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
      "token": {
        "username": "admin",
        "tokenUUID": "ab394518-6c24-4330-bb38-65c9ae1845c4",
        "originatingIP": "0:0:0:0:0:0:0:1",
        "version": 1.0,
        "expires": 1576667359617,
        "groups": [
        "Customer"
        ],
        "customerId": 101
      }
      }
      ```
 
* **Error Response:**

  * **Code:** 401 <br />
    **Content:** 
      ```
      {
      "SRAError":{
      "errorDesc":"Invalid username or password",
      "errorType":"business",
      "errorCode":"SRA-0003"
      }
      }
      ```
## Get Accounts

Get accounts of logged in users. 

* **URL**

  /sra/customers/{customer_id}/smilevoiceaccounts

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | customer_id           | Y        | customer's profile id               |
  
* **Query Params**

   None
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
      "accounts": [
        {
            "smileVoiceNo": "07020211495",
            "unitCreditConfig": [],
            "account": {
                "availableBalanceInCents": 1341100.0,
                "unitCredits": [
                    {
                        "purchaseDate": 1585737652000,
                        "endDate": 1588934452000,
                        "extTxId": "Split_5576841",
                        "priceInCents": 0.0,
                        "productInstanceId": 435539,
                        "currentUnitsRemaining": 5.24288E8,
                        "unitCreditInstanceId": 5576855,
                        "unitsAtStart": 5.24288E8,
                        "expiryDate": 1588934452000,
                        "unitType": "Byte",
                        "accountId": 1909000000,
                        "unitCreditSpecificationId": 523,
                        "availableUnitsRemaining": 5.24288E8,
                        "unitCreditDescription": "1.5GB Plan",
                        "name": "1GB DataPlan",
                        "saleLineId": 12569749,
                        "startDate": 1585737652000,
                        "info": "DaysGapBetweenStart=0\r\nToAccountId=1412002181\r\nNoKits=true\r\nSplit=true"
                    }
                ],
                "accountId": 1909000000,
                "currentBalanceInCents": 1341100.0,
                "reservations": [],
                "accountSummary": {
                    "resultsReturned": 0,
                    "periodSummaries": []
                },
                "accountHistory": {
                    "resultsReturned": 0,
                    "transactionRecords": []
                },
                "status": 8
            },
            "friendlyName": "Office"
            }
         ],
         "notifications": 0
      }
      ```
 
* **Error Response:**

  * **Code:** 404 <br />
    **Content:** 
      ```
      {
        "SRAError": {
          "errorDesc": "Customer not found",
          "errorType": "BUSINESS",
          "errorCode": "SRA-00013"
        }
      }
      ```
      
  * **Code:** 401 <br />
    **Content:** 
      ```
      {
        "SRAError": {
          "errorDesc": "Token is invalid or expired -- f13db56a-5078-48b6-a771-cb9d17546752",
          "errorType": "business",
          "errorCode": "SRA-0003"
        }
      }
      ```
## Get Account for smile number

Get accounts of logged in user for smile number. 

* **URL**

  /sra/customers/smilevoiceaccounts?smileVoiceNo={smile_number}

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | smile_number          | Y        | customers smile voice number        |
  
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "account_id": "1303000023",
        "customer_id": "520"
      }
      ```
 
* **Error Response:**

  * **Code:** 400 <br />
    **Content:** 
      ```
      {
        "errorDesc": "invalid smile voice number",
        "errorType": "BUSINESS",
        "errorCode": "SRA-0001"
      }
      ```
      
  * **Code:** 401 <br />
    **Content:** 
      ```
      {
        "SRAError": {
          "errorDesc": "Token is invalid or expired -- f13db56a-5078-48b6-a771-cb9d17546752",
          "errorType": "business",
          "errorCode": "SRA-0003"
        }
      }
      ```

## Redeem Voucher

Redeem a prepaid voucher for airtime or data/voice bundles. 

* **URL**

  /sra/accounts/{account_id}/redeemvoucher/{voucher_pin}

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | account_id            | Y        | customer's account number           |
  | voucher_pin           | Y        | prepaid voucher pin                 |
  
* **Query Params**

   None
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
          "expiryDate": 1601720308930,
          "unitCreditSpecificationId": 102,
          "prepaidStripId": 9,
          "valueInCents": 120000.0,
          "unitCreditName": "1GB data plan",
          "status": "RE" // RE - Redemed
      }
      ```
 
* **Error Response:**

  * **Code:** 400 <br />
    **Content:** 
      ```
      {
         "SRAError": {
           "errorDesc": "Sorry! The Voucher PIN entered could not be processed. Please contact Smile Customer Care on +234702044444 for assistance.",
           "errorType": "BUSINESS",
           "errorCode": "SRA-0013"
         }
      }
      ```
