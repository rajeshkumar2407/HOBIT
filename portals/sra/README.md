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
