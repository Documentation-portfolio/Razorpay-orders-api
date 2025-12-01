## Authentication

Username

Password

## Authorization

Basic Auth

Header format: `Basic base64token`

`base64token` is a base64 encoded string of `YOUR_KEY_ID:YOUR_KEY_SECRET`.

---

**Warning**: Invalid headers for authorization will result in authentication failures.

---
## Create an order

Create an order with details such as currency and amount.

## **Method**

POST

### **Endpoint Path**

POST [https://api.razorpay.com/v1/orders](https://api.razorpay.com/v1/orders)

### Headers

Content-Type: application/json

Authorization: Basic Auth

Header format: `Basic base64token`

`base64token` is a base64 encoded string of `YOUR_KEY_ID:YOUR_KEY_SECRET`.

## Request Parameters

**amount** integer

The payment amount related to the order. The payment amount is required to be in the smallest currency sub-unit.

**currency** string

The ISO code for the currency related to the order.

Default characters: 3

**receipt** string

The unique receipt number related to the order.

Maximum characters: 40

**notes** json object

The key-value pair used to store additional information about the entity.

Maximum key-value pairs: 15

Maximum characters for each key-value pair: 256

## Request Body

``` json
{
  "amount": 0,
  "currency": "INR",
  "receipt": "string",
  "notes": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  }
}

 ```

## Curl

```
curl -u [YOUR_KEY_ID]:[YOUR_KEY_SECRET] \
  -X POST https://api.razorpay.com/v1/orders \
  -H "Content-Type: application/json" \
  -d '{
        "amount": 5000,
        "currency": "INR",
        "receipt": "receipt#1",
        "notes": {
          "key1": "value3",
          "key2": "value2"
        }
      }'

 ```

## HTTP Status Codes

Order created successfully `201`

``` json
{
  "id": "string",
  "amount": 0,
  "amount_paid": 0,
  "amount_due": 0,
  "currency": "string",
  "status": "string",
  "receipt": "string",
  "created_at": 0,
  "notes": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  }
}

 ```

## Errors

**Authentication failed** `Error Status: 400`

- **Meaning**: The API credentials in the API request is different from the credentials on the Dashboard.
    
- **Reasons**: Different keys for test and live modes, or expired API key.
    
- **Fix**: Ensure the API key is active. Enter the API key without space before and after the key.
    

**The amount must be at least 1.00 INR.** `Error Status: 400`

- **Meaning**: The amount specified is less than the minimum amount.
    
- **Fix**: Enter an amount equal to or greater than 1.00 INR. Ensure the currency sub-units, such as paise for INR, is always greater than 100.
    

**The field name is required.** `Error Status: 400`

- **Meaning**: The mandatory field is missing.
    
- **Fix**: Ensure all mandatory fields and values are present.


## Fetch an order

Fetch an order with details such as currency and amount.

## **Method**

GET

### **Endpoint Path**

GET [https://api.razorpay.com/v1/orders/{order_id}]

### Headers

Content-Type: application/json

Authorization: Basic Auth

Header format: `Basic base64token`

`base64token` is a base64 encoded string of `YOUR_KEY_ID:YOUR_KEY_SECRET`.

## Query Parameters

**authorized** integer

Values:

- `1`: Retrieves orders for which payments have been authorized.
    
- `0`: Retrieves orders for which payments have not been authorized.

## Response Parameters

**id** string

The unique ID of the order.

**amount** integer

The payment amount related to the order. The payment amount is required to be in the smallest currency sub-unit.

**entity** string

The name of the entity. For this request, the entity is `order`.

**amount** integer

The amount for which the order was created.

**amount_paid** integer

The amount paid against the order.

**amount_due** integer

The amount pending against the order.

**currency** string

The ISO code for the currency related to the order.

Default characters: 3

**receipt** string

The unique receipt number related to the order.

Maximum characters: 40

## Curl

```
curl -u [YOUR_KEY_ID]:[YOUR_KEY_SECRET]\
-X GET https://api.razorpay.com/v1/orders?count=2&skip=1

 ```

## HTTP Status Codes

Order details `200`

``` json
{
  "id": "string",
  "amount": 0,
  "amount_paid": 0,
  "amount_due": 0,
  "currency": "string",
  "status": "string",
  "receipt": "string",
  "created_at": 0,
  "notes": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  }
}

 ```

## Errors

**The API Key or Secret provided is invalid** `Error Status: 400`

- **Meaning**: The API credentials in the API request is different from the credentials on the Dashboard.
    
- **Reasons**: Different keys for test and live modes, or expired API key.
    
- **Fix**: Ensure the API key is active. Enter the API key without space before and after the key.


## Fetch all payments for an order

Fetch all the payments made for an order.

## **Method**

GET

### **Endpoint Path**

GET [https://api.razorpay.com/v1/orders/{order_id}/payments]

### Headers

Content-Type: application/json

Authorization: Basic Auth

Header format: `Basic base64token`

`base64token` is a base64 encoded string of `YOUR_KEY_ID:YOUR_KEY_SECRET`.

## Path Parameters

**id** string

The unique ID of the order.

## Response Parameters

**id** string

The unique ID of the payment.

**amount** integer

The payment amount related to the order. The payment amount is required to be in the smallest currency sub-unit.

**entity** string

The type of the entity.

**currency** string

The ISO code for the currency related to the order.

Default characters: 3

**receipt** string

The unique receipt number related to the order.

Maximum characters: 40

**status** string

The status of the payment. The values are:

- `created`
    
- `authorized`
    
- `captured`
    
- `refunded`
    
- `failed`
    

**method** string

The payment method used for payment. The values are:

- `card`
    
- `netbanking`
    
- `wallet`
    
- `upi`
    
- `emi`
    

## Curl

```
curl -u [YOUR_KEY_ID]:[YOUR_KEY_SECRET]\
-X GET https://api.razorpay.com/v1/orders/order_DaaS6LOUAASb7Y/payments \

 ```

## HTTP Status Codes

List of payments for the order `200`

``` json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "amount": 0,
      "currency": "string",
      "status": "string",
      "order_id": "string",
      "method": "string",
      "created_at": 0
    }
  ]
}

 ```

## Capture a payment

Capture an authorized payment to complete the transaction. Ensure the status of the payment is `authorized.`

## **Method**

POST

### **Endpoint Path**

POST /payments/{payment_id}/capture

### Headers

Content-Type: application/json

Authorization: Basic Auth

Header format: `Basic base64token`

`base64token` is a base64 encoded string of `YOUR_KEY_ID:YOUR_KEY_SECRET`.

## Path Parameters

**payment_id** string

The unique ID of a payment you want to capture.

## Request Parameters

**amount** integer (required)

The payment amount you want to capture.

**currency** string

The ISO code for the currency related to the payment.

Default characters: 3

## Request Body

``` json
{
  "amount": 0,
  "currency": "INR"
}

 ```

## Curl

``` json
curl -u [YOUR_KEY_ID]:[YOUR_KEY_SECRET] \
-H 'content-type: application/json' \
-X POST https://api.razorpay.com/v1/payments/pay_29QQoUBi66xm2f/capture \
-d '{
  "amount": 1000,
  "currency": "INR"
}'

 ```

## HTTP Status Codes

Payment captured successfully `200`

``` json
{
  "id": "string",
  "amount": 0,
  "currency": "string",
  "status": "string",
  "order_id": "string",
  "method": "string",
  "created_at": 0
}

 ```

## Errors

**Capture amount is required to be equal to the amount authorized** `Error Status: 400`

- **Meaning**: The capture amount is different from the authorized amount.
    
- **Fix**: Ensure the captured amount is equal to the authorized amount..
    

**The requested URL is not found on the server** `Error Status: 400`

- **Meaning**: The URL is incorrect.
    
- **Fix**: Use the correct URL.


## Create a refund

Create a full or a partial refund against a captured payment.

## **Method**

POST

### **Endpoint Path**

POST /refunds

### Headers

Content-Type: application/json

Authorization: Basic Auth

Header format: `Basic base64token`

`base64token` is a base64 encoded string of `YOUR_KEY_ID:YOUR_KEY_SECRET`.

## Path Parameters

**payment_id** string (required)

The unique ID of a payment you want to capture.

## Request Parameters

**amount** integer (required)

The payment amount you want to refund.

- For a partial refund, enter a value less than the payment amount.
    
- For a full refund, enter a value equal to the payment amount.
    

**Warning**: Ensure the last decimal number of the `amount` is `0`. For example, if you want to refund 99.991 Rupees, then ensure the amount parameter is `99990`.

**speed** string

The speed of processing the refund. The valid values are:

- `normal` (default)
    
- `optimum`
    

**notes** json object

Key-value pair to store additional information about the redund.

Maximum key-value pairs: 15

## Request Body

``` json
{
  "payment_id": "string",
  "amount": 0,
  "speed": "normal",
  "notes": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  }
}

 ```

## Curl

``` json
curl -u [YOUR_KEY_ID]:[YOUR_KEY_SECRET] \
-X POST https://api.razorpay.com/v1/payments/pay_29QQoUBi66xm2f/refund \
-H 'Content-Type: application/json' \
-d '{
  "amount": 500100
}'

 ```

## HTTP Status Codes

Refund created `201`

``` json
{
  "id": "string",
  "amount": 0,
  "currency": "string",
  "status": "string",
  "created_at": 0
}

 ```

## Errors

**{payment_id} is not a valid ID** `Error Status: 400`

- **Meaning**: The `payment_id` is invalid.
    
- **Fix**: Use the correct `payment_id`.
    

**The requested URL is not found on the server** `Error Status: 400`

- **Meaning**: The URL is incorrect.
    
- **Fix**: Use the correct URL.
