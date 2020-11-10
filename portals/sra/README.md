# Smile REST API

- [Smile REST API](#smile-rest-api)
    - [Registration](#registration)
    - [Login](#login)
    - [Get Customer Details](#get-customer-details)
    - [Get Customer Without Token](#get-customer-without-token)
    - [Get Customer KYC Deatils](#get-customer-kyc-details)
    - [Change Password](#change-password)
    - [Forget Password](#forget-password)
    - [Get Accounts](#get-accounts)
    - [Get Account for smile number](#get-account-for-smile-number)
    - [Get Account History](#get-account-history)
    - [Download Account History](#download-account-history)
    - [Get Bundles](#get-bundles)
    - [Get Extra Bundles](#get-extra-bundles)
    - [Redeem Voucher](#redeem-voucher)
    - [Sale Airtime Using Payment Gateway](#sale-airtime-using-payment-gateway)
    - [Sale Bundle Using Payment Gateway](#sale-bundle-using-payment-gateway)
    - [Get Sale Status](#get-sale-status)
    - [Sale Bundle Using Airtime](#sale-bundle-using-airtime)
    - [Share Airtime](#share-airtime)
    - [Share Data](#share-data)
    - [Device Details](#device-details)
    - [Get Notifications](#get-notifications)
    - [Update Profile Photo](#update-profile-photo)
    - [Get Profile Photo](#get-profile-photo)


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
    "customerId":12345, // It should be 0 for guest
    "identity":"09876485982", //Mobile number used for OTP
    "key":"594432", // OTP Code. Valid for 15 Min
    "newPassword":"test123", // Not required if customerId is 0
    "confirmPassword":"test123" //Not required if customerId is 0
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
## Get Customer Details

Get personal details for loged in user. 

* **URL**

  /sra/customers/{identifier}?by={id | email | username}

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | identifier            | Y        | customer's id or email or username  |
  
* **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | by                    | Y        | identifier type. ie id or username  |
  | verbosity             | N        | CUSTOMER_PHOTO for customer's photo |
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "customer": {
        "lastName": "kumar",
        "addresses": [],
        "gender": "M",
        "kycStatus": "V", // V:Verified, U:Unverifies, P:Pending
        "createdDateTime": 1418491489000,
        "identityNumberType": "",
        "language": "en",
        "passportExpiryDate": "",
        "mothersMaidenName": "MMN_32450",
        "products": [],
        "customerStatus": "AC",
        "createdByCustomerProfileId": 7054,
        "emailAddress": "rajesh.kumar@smilecoms.com",
        "identityNumber": "32450",
        "customerId": 32450,
        "SSOIdentity": "raj",
        "accountManagerCustomerProfileId": 7054,
        "optInLevel": 7,
        "alternativeContact1": "11132450",
        "dateOfBirth": "19860724",
        "alternativeContact2": "22232450",
        "classification": "customer",
        "outstandingTermsAndConditions": [],
        "version": 6,
        "firstName": "rajesh",
        "nationality": "NG",
        "warehouseId": "",
        "customerPhotographs": [],
        "middleName": "",
        "securityGroups": [
            "Customer",
            "TPGW"
        ],
        "customerRoles": []
        }
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
      
## Get Customer Without Token

Get personal details of a customer without using token. 

* **URL**

  /sra/smilecustomer?identity={email | username | smielnumber}

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

   None
  
* **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | identity              | Y        |  username or email or smile number  |
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "customerId": 32450,
        "title": "",
        "firstName": "rajesh",
        "middleName": "",
        "lastName": "kumar",
        "alternativeContact1": "11132450",
        "alternativeContact2": "22232450",
        "emailAddress": "rajesh.kumar@smilecoms.com"
      }
      ```
 
* **Error Response:**

  * **Code:** 404 <br />
    **Content:** 
      ```
      {
        "SRAError": {
          "errorDesc": "Customer not found",
          "errorType": "business",
          "errorCode": "SRA-0053"
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
      
## Get Customer KYC Details

Get customer's KYC details. 

* **URL**

  /sra/customers/{customer_id}/kycdetail

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
          "emailVerified": "Y",
          "titleVerified": "Y",
          "nationalityVerified": "Y",
          "dobVerified": "Y",
          "facialPitureVerified": "Y",
          "customerId": 32450,
          "physicalAddressVerified": "Y",
          "mobileVerified": "N",
          "fingerPrintVerified": "N",
          "genderVerified": "Y",
          "nameVerified": "Y",
          "validIdCardVerified": "Y"
      }
      ```
 
* **Error Response:**

  * **Code:** 404 <br />
    **Content:** 
      ```
      {
        "SRAError": {
          "errorDesc": "Customer not found",
          "errorType": "business",
          "errorCode": "SRA-0053"
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

## Change Password

Reset or change customers password. 

* **URL**

  /sra/customers/{customer_id}/changepassword

* **Method:**

  `PUT`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | customer_id           | Y        | customer id                         |
  
* **Query Params**

  None
  
* **Request Body:**

   ```
   {
     "newPassword": "newpassword",
     "confirmPassword": "newpassword"
   }
   ```
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "done": true
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
## Forget Password

API to rest password using OTP. 

* **URL**

  /sra/changepwdbyotp

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  None
  
* **Query Params**

  None
  
* **Request Body:**

   ```
   {
    "customerId":1394431,
    "identity":"09876485982", //OTP mobile number
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
        "done": true
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
      
## Share Data
API to share data bundles with other accounts. 

* **URL**

  /sra/accounts/sharedata

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  None
  
* **Query Params**

  None
  
* **Request Body:**

   ```
   {
	"sourceAccountId":2002000000,
	"targetAccounts":[1811000022],
	"unitCreditInstanceId":2883928,
	"units":120400
   }
   ```
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "availableBalanceInCents": 0.0,
        "accountId": 2002000000,
        "currentBalanceInCents": 0.0,
        "unitCreditInstances": [
        {
            "unitType": "Byte",
            "accountId": 2002000000,
            "unitCreditSpecificationId": 456,
            "availableUnitsRemaining": 1.610612736E9,
            "name": "Data Plan - 1.5GB",
            "productInstanceId": 435625,
            "currentUnitsRemaining": 1.610612736E9,
            "unitCreditInstanceId": 5576605
        },
        {
            "unitType": "SMS",
            "accountId": 2002000000,
            "unitCreditSpecificationId": 502,
            "availableUnitsRemaining": 1000.0,
            "name": "Free Onnet SMS",
            "productInstanceId": 435625,
            "currentUnitsRemaining": 1000.0,
            "unitCreditInstanceId": 5576673
        }
        ]
      }
      ```
      
  * **Code:** 404 <br />
    **Content:** 
      ```
      {
        "SRAError": {
        "errorDesc": "invalid account id",
        "errorType": "business",
        "errorCode": "SRA-0003"
        }
      }
      ```    
  
## Share Airtime
API to share airtime with other accounts. 

* **URL**

  /sra/accounts/sharedata

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  None
  
* **Query Params**

  None
  
* **Request Body:**

   ```
   {
	"sourceAccountId":1811000020,
	"targetAccountId":1811000022,
	"amountInCents":1000
   }
   ```
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
       "availableBalanceInCents": 0.0,
       "accountId": 2002000000,
       "currentBalanceInCents": 0.0,
       "status": 8
      }
      ```
      
  * **Code:** 404 <br />
    **Content:** 
      ```
      {
        "SRAError": {
        "errorDesc": "invalid account id",
        "errorType": "business",
        "errorCode": "SRA-0003"
        }
      }
      ```   
  
## Get Account History
Get accounts usage history. 

* **URL**

  /sra/accounts/{account_no}/allhistory?dateFrom={from_date}&dateTo={to_date}&resultLimit={record_size}&recordType={record_type}

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | account_no            | Y        | customer's account no               |
  
* **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | dateFrom              | N        | record start date                   |
  | dateTo                | N        | record end date                     |
  | resultLimit           | N        | number of records                   |
  | recordType            | N        | type of records                     |
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
      "resultsReturned": 3,
      "transactionRecords": [
         {
            "amountInCents": 0.0,
            "unitCreditBaselineUnits": 0.0,
            "endDate": "2020-06-08T19:45:05.000+02:00",
            "extTxId": "UNIQUE-UC-SaleLine-12570211",
            "unitCreditUnits": 0.0,
            "destination": "",
            "ipAddress": "",
            "description": "Purchase 5GB Data Plan",
            "serviceInstanceId": 0,
            "source": "",
            "serviceInstanceIdentifier": "",
            "totalUnits": 0.0,
            "transactionType": "txtype.uc.purchase",
            "accountId": 1303000023,
            "transactionRecordId": 1830762761,
            "accountBalanceRemainingInCents": 587232.27,
            "location": "",
            "startDate": "2020-06-08T19:45:05.000+02:00",
            "info": "SaleLineId=12570211\r\nnull",
            "status": "FI",
            "termCode": ""
          },
          {
            "amountInCents": 0.0,
            "unitCreditBaselineUnits": 0.0,
            "endDate": "2020-06-08T19:45:04.000+02:00",
            "extTxId": "UNIQUE-UC-SaleLine-12570211",
            "unitCreditUnits": 0.0,
            "destination": "",
            "ipAddress": "",
            "description": "Purchase Free Onnet SMS (5578083)",
            "serviceInstanceId": 0,
            "source": "",
            "serviceInstanceIdentifier": "",
            "totalUnits": 0.0,
            "transactionType": "txtype.uc.purchase",
            "accountId": 1303000023,
            "transactionRecordId": 1830762759,
            "accountBalanceRemainingInCents": 587232.27,
            "location": "",
            "startDate": "2020-06-08T19:45:04.000+02:00",
            "info": "SaleLineId=12570211\r\nSMS_VAT",
            "status": "FI",
            "termCode": ""
          },
          {
            "amountInCents": 0.0,
            "unitCreditBaselineUnits": 0.0,
            "endDate": "2020-06-08T19:45:04.000+02:00",
            "extTxId": "UNIQUE-UC-SaleLine-12570211",
            "unitCreditUnits": 0.0,
            "destination": "",
            "ipAddress": "",
            "description": "Purchase Free Onnet Voice (5578081)",
            "serviceInstanceId": 0,
            "source": "",
            "serviceInstanceIdentifier": "",
            "totalUnits": 0.0,
            "transactionType": "txtype.uc.purchase",
            "accountId": 1303000023,
            "transactionRecordId": 1830762757,
            "accountBalanceRemainingInCents": 587232.27,
            "location": "",
            "startDate": "2020-06-08T19:45:04.000+02:00",
            "info": "SaleLineId=12570211\r\nVoice_VAT",
            "status": "FI",
            "termCode": ""
          }
        ]
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
## Download Account History

Download accounts usage history. 

* **URL**

  /sra/accounts/{account_no}/downloadhistory?dateFrom={from_date}&dateTo={to_date}&resultLimit={record_size}&recordType={record_type}

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | account_no            | Y        | customer's account no               |
  
* **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | dateFrom              | N        | record start date                   |
  | dateTo                | N        | record end date                     |
  | resultLimit           | N        | number of records                   |
  | recordType            | N        | type of records                     |
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    
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

## Get Bundles

Get list of all the bundles available for the logged in user. 

* **URL**

  /sra/catalogs/bundles

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **Query Params**

   None  
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      [
      {
        "itemNumber": "BUNPCG1001",
        "purchaseRoles": "Customer",
        "configuration": "",
        "priceInCents": 0.0,
        "filterClass": "NeverAllowFilterClass",
        "description": "AlwaysOn Internet with speed up to 3Mbps valid 30 days",
        "units": 2.0E11,
        "priority": 0,
        "availableFrom": 1562191200000,
        "availableTo": 1924898400000,
        "unitType": "Byte",
        "usableDays": 30,
        "wrapperClass": "ContainerUnitCredit",
        "unitCreditSpecificationId": 515,
        "name": "Freedom 3Mbps",
        "productServiceMappings": [
            {
                "productSpecificationId": 0,
                "serviceSpecificationId": 18
            }
        ],
        "validityDays": 37
       }
      ]
      ```
 
* **Error Response:**
      
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
      
## Get Extra Bundles

Get list of extra bundles available for specific bundle. 

* **URL**

  /sra/catalogs/extrabundles/{ucID}

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **Query Params**

   None  
   
*  **Path Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | ucID                  | Y        | bundle specification id             |
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      [
      {
        "unitType": "Byte",
        "itemNumber": "BUN9106",
        "usableDays": 30,
        "unitCreditSpecificationId": 437,
        "priceInCents": 255952.0,
        "name": "5GB UPSize 30 days",
        "description": "5GB UPSize 30 days",
        "units": 5.36870912E9,
        "validityDays": 37
      },
      {
        "unitType": "Byte",
        "itemNumber": "BUN9105",
        "usableDays": 30,
        "unitCreditSpecificationId": 436,
        "priceInCents": 153571.0,
        "name": "3GB UPSize 30 days",
        "description": "3GB UPSize 30 days",
        "units": 3.221225472E9,
        "validityDays": 37
      }
      ]
      ```
 
* **Error Response:**
      
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
        "usableDays": 365, // Validity period in Days
        "unitCreditSpecificationId": 103, // 0 for Airtime voucher
        "prepaidStripId": 11,
        "units": 1.073741824E10, // In bytes for Data bundles. 0.0 for airtime voucher
        "valueInCents": 120000.0,
        "unitCreditName": "10GB Anytime",
        "status": "RE"
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
      
## Sale Airtime Using Payment Gateway

API to buy airtime using payment gateways. 

* **URL**

  /sra/sales/airtime

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

   None
  
* **Query Params**

   None
  
* **Request Body:**

   ```
   {
	"sale":{
		"saleTotalCentsIncl":10000.00,
		"saleLines":[{"inventoryItem": {"serialNumber":"AIRTIME","description":"AIRTIME","itemNumber":"AIR1004","warehouseId":"","priceInCentsIncl":100.00},"quantity":100}],
	    "paymentMethod":"Payment Gateway",
	    "paymentGatewayCode":"Paystack",
	    "recipientCustomerId":50891,
	    "recipientAccountId":1504000802,
	    "warehouseId":""
    }
   }
   ```
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
        {
        "sale": {
        "saleId": 11028399,
        "paymentGatewayLastPollDate": 0,
        "channel": "41WABA01",
        "tenderedCurrency": "NGN",
        "saleDate": 1582640641000,
        "amountTenderedCents": 0.0,
        "salesPersonCustomerId": 1,
        "saleLines": [
            {
                "lineTotalDiscountOnInclCents": 0.0,
                "inventoryItem": {
                    "itemNumber": "AIR1004",
                    "boxSize": 0,
                    "priceInCentsExcl": 93.02,
                    "serialNumber": "AIRTIME",
                    "warehouseId": "",
                    "priceInCentsIncl": 100.0,
                    "description": "Airtime",
                    "currency": "NGN",
                    "stockLevel": 100
                },
                "quantity": 100,
                "lineId": 12569575,
                "parentLineId": 0,
                "lineTotalCentsIncl": 10000.0,
                "lineTotalCentsExcl": 9302.33,
                "subSaleLines": [],
                "lineTotalDiscountOnExclCents": 0.0,
                "returns": [],
                "lineNumber": 1,
                "provisioningData": ""
            }
        ],
        "paymentGatewayPollCount": 0,
        "recipientAccountId": 1504000802,
        "paymentGatewayURL": "https://checkout.paystack.com/3m7qx3k91ss6nie",
        "tillId": "",
        "landingURL": "/PaymentGateway.action?processBankTransaction&saleId=11028399",
        "purchaseOrderData": "",
        "paymentGatewayCode": "Paystack",
        "taxExempt": false,
        "extTxId": "30eda74a-ab02-4c3a-893b-47b2e47821a4",
        "promotionCode": "",
        "paymentGatewayURLData": "POST: amount=10000,callback_url=http://41.169.133.180:8004/scp/Login.action,
        "creditAccountNumber": "ACC006",
        "recipientCustomerId": 50891,
        "organisationName": "",
        "recipientOrganisationId": 0,
        "withholdingTaxRate": 0.0,
        "saleTotalCentsIncl": 10000.0,
        "salesPersonAccountId": 1711000000,
        "contractSaleId": 0,
        "paymentGatewayNextPollDate": 0,
        "warehouseId": "",
        "contractId": 0,
        "paymentMethod": "Payment Gateway",
        "extraInfo": "",
        "saleTotalTaxCents": 697.67,
        "status": "PP"
        }
       }
      ```
 
* **Error Response:**

  * **Code:** 400 <br />
    **Content:** 

## Sale Bundle Using Payment Gateway

API to buy data bundles using payment gateway. 

* **URL**

  /sra/sales/bundle?gatewayCode={payment_gateway_code}

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

   None
  
* **Query Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | gatewayCode           | Y        | payment gateway code                |
  | recipientPhoneNumber  | N        | customer's phone no                 |
  | extraUccId            | N        | extra bundle ID                     |
  
* **Request Body:**

   ```
   {
    "accountId":1303000023,
    "unitCreditSpecificationId":587,
    "unitCreditName":"40GB Anytime Plan 37 Days",
    "itemNumber":"BUN1040", 
    "productInstanceId":0,
    "numberToPurchase":1,
    "paymentMethod":"Payment Gateway",
    "creditAccountNumber":"1303000023"
   }
   ```
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "sale": {
        "saleId": 32541803,
        "paymentGatewayLastPollDate": 0,
        "channel": "41WPAY01",
        "tenderedCurrency": "NGN",
        "saleDate": 1585054665000,
        "amountTenderedCents": 0.0,
        "salesPersonCustomerId": 1,
        "saleLines": [
            {
                "lineTotalDiscountOnInclCents": 0.0,
                "inventoryItem": {
                    "itemNumber": "BUN1050",
                    "boxSize": 0,
                    "priceInCentsExcl": 3427906.98,
                    "serialNumber": "",
                    "warehouseId": "",
                    "priceInCentsIncl": 3685000.0,
                    "description": "Data Bundle (50GB)",
                    "currency": "NGN",
                    "stockLevel": 1
                },
                "quantity": 1,
                "lineId": 35627023,
                "parentLineId": 0,
                "lineTotalCentsIncl": 3685000.0,
                "lineTotalCentsExcl": 3427906.98,
                "subSaleLines": [],
                "lineTotalDiscountOnExclCents": 0.0,
                "returns": [],
                "lineNumber": 1,
                "provisioningData": ""
            }
        ],
        "paymentGatewayPollCount": 0,
        "recipientAccountId": 2003009622,
        "paymentGatewayURL": "https://checkout.paystack.com/gzx6a46qocfgnpp",
        "tillId": "",
        "landingURL": "/PaymentGateway.action?processBankTransaction&saleId=32541803",
        "purchaseOrderData": "",
        "paymentGatewayCode": "Paystack",
        "taxExempt": false,
        "extTxId": "d2c2cad3-7ceb-40d8-b8b3-9b0da87825c5",
        "promotionCode": "",
        "paymentGatewayURLData": "POST: amount=3685000,callback_url=https://smile.com.ng/scp/Login.action",
        "creditAccountNumber": "PAY001",
        "recipientCustomerId": 837195,
        "organisationName": "",
        "recipientOrganisationId": 0,
        "withholdingTaxRate": 0.0,
        "saleTotalCentsIncl": 3685000.0,
        "salesPersonAccountId": 1711004212,
        "contractSaleId": 0,
        "paymentGatewayNextPollDate": 0,
        "warehouseId": "",
        "contractId": 0,
        "paymentMethod": "Payment Gateway",
        "extraInfo": "",
        "saleTotalTaxCents": 257093.02,
        "status": "PP"
       }
      }
      ```

## Sale Bundle Using Airtime

API to buy data bundles using airtime. 

* **URL**

  /sra/sales/bundle

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

   None
  
* **Query Params**

   None
  
* **Request Body:**

   ```
   {
    "accountId":1303000023,
    "unitCreditSpecificationId":587,
    "unitCreditName":"40GB Anytime Plan 37 Days",
    "itemNumber":"BUN1040", 
    "productInstanceId":0,
    "numberToPurchase":1,
    "paymentMethod":"Airtime",
    "creditAccountNumber":"1303000023"
   }
   ```
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "done": "TRUE"
      }
      ```

## Device Details

Get details of all the available devices in selected customers account. 

* **URL**

  /sra/accounts/{account_id}/products

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | account_id            | Y        | customer's account number           |
  
* **Query Params**

   None
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      [
      {
        "serialNumber": "2993929994938112",
        "lastUsedDate": "2020-09-07 12:23:20",
        "description": "Sim Cards",
        "purchageDate": "2020-07-01 09:23:32",
        "activationDate": "2020-07-01 12:30:40",
        "deviceName": "SIM"
      },
      {
        "serialNumber": "2993929994938112",
        "lastUsedDate": "2020-09-07 12:23:20",
        "description": "Mobile Router Franklin B711 LTE",
        "purchageDate": "2020-07-01 09:23:32",
        "activationDate": "2020-07-01 12:30:40",
        "deviceName": "MIF6000"
      }
      ]
      ```
 
* **Error Response:**

  * **Code:** 400 <br />
    **Content:** 
      ```
      TODO
      ```

## Get Notifications

Get list of in-app notifications for logged in user. 

* **URL**

  /sra/customers/{customer_id}/notification

* **Method:**

  `GET`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | customer_id           | Y        | customer profile id                 |
  
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      [
      {
        "date": "2020-09-10 13:30:15",
        "customerId": 520,
        "notificationId": "e5df3aa3-9d73-47c2-9c0f-f59f4f9b55a3",
        "message": "20% of your 500MB data plan is left on smile number 07795329934 stay online and upgrade to 2.5 GB FlexiDaily for only N500",
        "title": "Data Usage Alert",
        "type": "recharge",
        "info": "https://smile.com.ng/",
        "status": "UR"
      },
      {
        "date": "2020-09-09 17:03:31",
        "customerId": 520,
        "notificationId": "f51ee645-ceff-4933-8365-c13e4fde6207",
        "message": "20% of your 500MB data plan is left on smile number 07795329934 stay online and upgrade to 2.5 GB FlexiDaily for only N500",
        "title": "Data Usage Alert",
        "type": "recharge",
        "info": "https://smile.com.ng/",
        "status": "UR"
      }
      ]
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

## Update Profile Photo

update customer's profile photo. provide the 'file' parameter as attachement in form date. 

* **URL**

  /sra/customers/{customer_id}/uploadprofilephoto

* **Method:**

  `POST`
    
*  **Header Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | X-token               | Y        | authentication token                |
  | Content-Type          | Y        | multipart/form-data                 |

*  **URL Params**

  | Parameter             | Required | Description                         |
  | --------------------- |:--------:| -----------------------------------:|
  | customerid            | Y        | customer's profile id               |
  
* **Query Params**

   None
  
* **Request Body:**

   None
   
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      ```
      {
        "done": true
      }
      ```
      
## Get Profile Photo

download customer's profile photo. 

* **URL**

  /sra/customers/{customer_id}/downloadprofilephoto

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
      pfofile picture in application/octet-stream response type
      ```
