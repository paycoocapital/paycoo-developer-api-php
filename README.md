# paycoo-developer-api-php
Welcome to the Official Paycoo Fintech Payment Gateway Solutions Developer API Documentation

Welcome to the Paycoo Payment Gateway Solutions Developer API Documentation. This comprehensive guide will empower you to seamlessly integrate our advanced payment gateway into your website, enhancing your customers’ payment experience and enabling efficient transaction processing. The Paycoo Developer API is designed for developers and entrepreneurs who seek simplicity, security, and reliability in their payment processing solutions.

Introduction
  The Paycoo Developer API allows you to seamlessly integrate the Offline Payment Gateway Solutions into your website, enabling secure and efficient debit and credit card transactions even without an internet connection. With our API, you can initiate payments, check payment statuses, and even process refunds, all while ensuring a smooth and streamlined payment experience for your customers.

Prerequisites
Before you begin integrating the Paycoo Developer API, make sure you have:

1. An active Paycoo merchant account.
2. Basic knowledge of API integration and web development with PHP & Laravel.
3. A secure and accessible web server to handle API requests.

Authentication
To access the Paycoo Developer API, you’ll need an API key. You can obtain your API key by logging in to your Paycoo merchant account and navigating to the API section. Collect Client/Primary Key & Secret Key Carefully. Keep your API key confidential and do not share it publicly.

If you don't have any merchant account please Register to continue

Base URL
The base URL for API requests is:

For PRODUCTION Mode: https://paycooofflinenet/paycooofflinenet/pay/api/v1
For SANDBOX Mode: https://paycooofflinenet/qrpay/pay/sandbox/api/v1

Get Access Token
Get access token to initiates payment transaction.

Endpoint: POST {{base_url}}/authentication/token
Parameter	Type	Comments
client_id	string	Enter merchant API client/primary key
secret_id	string	Enter merchant API secret key


Just request to that endpoint with all parameter listed below:
Request Example (guzzle)

<?php
require_once('vendor/autoload.php');
$client = new \GuzzleHttp\Client();
$response = $client->request('POST', '{{base_url}}/authentication/token', [
'json' => [
  'client_id' => 'tRCDXCuztQzRYThPwlh1KXAYm4bG3rwWjbxM2R63kTefrGD2B9jNn6JnarDf7ycxdzfnaroxcyr5cnduY6AqpulRSebwHwRmGerA',
  'secret_id' => 'oZouVmqHCbyg6ad7iMnrwq3d8wy9Kr4bo6VpQnsX6zAOoEs4oxHPjttpun36JhGxDl7AUMz3ShUqVyPmxh4oPk3TQmDF7YvHN5M3',
 ],
'headers' => [
  'accept' => 'application/json',
  'content-type' => 'application/json',
 ],
]);
echo $response->getBody();

**Response: SUCCESS (200 OK)**
{
 "message": {
 "code": 200,
 "success": [
  "SUCCESS"
 ]
},
"data": {
 "access_token":"nyXPO8Re5SXP1c5gMqHbW6DQ5BfQdbYGpuWVjEQAP76SUT7YfdngoFzDGSNHTvmzq8AjPRrCyzxzukrJvOlSSwtAPAqjvAQJdse4YOnlHasD3vg6EYg6qyKxSiHeXBoRluD2NbZzxN3sAYVqd9q1XCAl7oaW3BbJl2ktEQWBUuNYMZPQaDyNEGwxoY389TCNJvxVcroveYxPJkYANvnaxOy16aE9Qp6EBClSjvK17WR3cJupTXlUhgw9ddpv1gDSlbDJvzKutrQX7XJqwk1GW1Dm6aK4PTn1D4mvMVqiOqQKigTzcEi2KPQnkoM86ONw3X8SxttFOfesdSwxKJMXuQpdnFHOjo",
 "expire_time": 600
},
"type": "success"
}

**Response: ERROR (400 FAILED)**
{
 "message": {
 "code": 400,
 "error": [
  "Invalid secret ID"
 ]
},
"data": [],
"type": "error"
}

Initiate Payment
Initiates a new payment transaction.

Endpoint: POST {{base_url}}/payment/create
Parameter	Type	Details
amount	decimal	Your Amount , Must be rounded at 2 precision.
currency	string	Currency Code, Must be in Upper Case (Alpha-3 code)
return_url:	string	Enter your return or success URL
cancel_url:	string (optional)	Enter your cancel or failed URL
custom:	string (optional)	Transaction id which can be used your project transaction

Request Example (guzzle)

<?php
require_once('vendor/autoload.php');
$client = new \GuzzleHttp\Client();
$response = $client->request('POST', '{{base_url}}/payment/create', [
'json' => [
  'amount' => '100.00',
  'currency' => 'USD',
  'return_url' => 'www.example.com/success',
  'cancel_url' => 'www.example.com/cancel',
  'custom' => '123456789ABCD',
 ],
'headers' => [
  'Authorization' => 'Bearer {{access_token}}',
  'accept' => 'application/json',
  'content-type' => 'application/json',
 ],
]);
echo $response->getBody();

**Response: SUCCESS (200 OK)**
{
 "message": {
 "code": 200,
 "success": [
  "CREATED"
 ]
},
"data": {
 "token": "2zMRmT3KeYT2BWMAyGhqEfuw4tOYOfGXKeyKqehZ8mF1E35hMwE69gPpyo3e",
 "payment_url": "www.example.com/pay/sandbox/v1/user/authentication/form/2zMRmT3KeYT2BWMAyGhqEfuw4tOYOfGXKeyKqehZ8mF1E35hMwE69gPpyo3e",
},
"type": "success"
}

**Response: ERROR (403 FAILED)**
{
 "message": {
 "code": 403,
 "error": [
  "Requested with invalid token!"
 ]
},
"data": [],
"type": "error"
}

Check Payment Status
Checks the status of a payment.


**Response: SUCCESS (200 OK)**
{
 "message": {
 "code": 200,
 "success": [
  "SUCCESS"
 ]
},
"data": {
 "token": "2zMRmT3KeYT2BWMAyGhqEfuw4tOYOfGXKeyKqehZ8mF1E35hMwE69gPpyo3e",
 "trx_id": "BP2c7sAvw75MTlrP",
 "payer": {
  "username": "testuser",
  "email": "user@paycoo.net"
 },
 "custom": "123456789ABCD"
},
"type": "success"
}


Response Codes
Paycoo API responses include standard HTTP status codes to indicate the success or failure of a request. Successful responses will have a status code of <strong>200 OK</strong>, while various error conditions will be represented by different status codes along with error messages in the response body.

Error Handling
In case of an error, the API will return an error response containing a specific error code 400, 403 Failed and a user-friendly message. Refer to our API documentation for a comprehensive list of error codes and their descriptions.

Best Practices
To ensure a smooth integration process and optimal performance, follow these best practices:

Use secure HTTPS connections for all API requests.
Implement robust error handling to handle potential issues gracefully.
Regularly update your integration to stay current with any API changes or enhancements.


Examples
For code examples and implementation guides, please refer to the “Examples” section on our developer portal. Go to GitHub Repository


Remember, our goal is to empower your business with a robust and efficient payment solution. If you have any additional questions or concerns, feel free to explore our developer portal or contact our support team.







 
