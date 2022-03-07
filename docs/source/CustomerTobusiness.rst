Customer to business (C2B)
--------------------------

C2B transaction requests are sent to the API for initiating the USSD PIN
prompt to the customer’s handset. Prior to the prompt, Mobile Money
account verifications are performed such as account status, balance
check etc. Following successful checks and PIN prompt entering the
transaction gets validated. As a result, the callback outcome request is
received and in case of success the considered service can be offered.

The process of transaction validation can take up to few minutes
(depending on MNO pending transaction expiration policy). In case of
technical challenges transactions can remain in pending state for longer
periods and are subject to manual reconciliation between UbiqPay and
MNO.

The callbacks with transaction outcomes are directly forwarded to
partner’s system. If partner doesn’t respond or responds with HTTP code
different than 200, a retry mechanism will be activated. This mechanism
is repeating the same callback request until proper response is received
or until it reaches time limit of two days. The retries are being
executed periodically with the laps of exponentially increasing number
of 5 seconds. Meaning, first retry will be initiated 5 seconds after the
final outcome of the transaction is known, second retry will be sent 25
seconds after the previous retry, third retry will be sent 125 seconds
after the previous one and so on.

The interaction between the partner, the UbiqPay system, the MNO system
and the customer is presented on the below diagram.

.. image:: vertopal_44c9d78e0d674f7497f174c873b751ac/media/image2.png
   :width: 6.5in
   :height: 5.22917in

requestC2B 
~~~~~~~~~~

HTTPS POST request sent from partner’s system to UbiqPay in order to
debit customer’s account.

**Path**

/momo/c2b

**Request**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| confirmC2BUrl         | String  | Partner’s URL for          |
|                       |         | receiving transaction      |
|                       |         | outcome callback           |
|                       |         | notification               |
+-----------------------+---------+----------------------------+
| extra                 | String  | Additional information     |
|                       |         | (description, email        |
|                       |         | address, account ID, …)    |
+-----------------------+---------+----------------------------+

Example:

**Response**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| status                | String  | Transaction status label   |
|                       |         | (INITIATING, INIT_SUCCESS, |
|                       |         | INIT_UNKNOWN, INIT_ERROR,  |
|                       |         | SUCCESSFUL, ERROR,         |
|                       |         | UNKNOWN)                   |
+-----------------------+---------+----------------------------+
| message               | String  | Acknowledgment status      |
|                       |         | description                |
+-----------------------+---------+----------------------------+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL            |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| transactionId         | String  | Unique transaction         |
|                       |         | identifier                 |
+-----------------------+---------+----------------------------+
| extra                 | String  | Additional information     |
+-----------------------+---------+----------------------------+
| paymentInstructions   | String  | Payment instructions       |
+-----------------------+---------+----------------------------+

Example:

Transaction in error:

For the list of supported error codes, see Annex, section 1.

confirmC2B 
~~~~~~~~~~

HTTPS POST request sent from UbiqPay system to the partner in order to
confirm the outcome of the transaction.

This request will be retried according to the previously described retry
mechanism.

**Path**

To be determined by partner.

**Request**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| status                | String  | Transaction status label   |
|                       |         | (INITIATING, INIT_SUCCESS, |
|                       |         | INIT_UNKNOWN, INIT_ERROR,  |
|                       |         | SUCCESSFUL, ERROR,         |
|                       |         | UNKNOWN)                   |
+-----------------------+---------+----------------------------+
| message               | String  | Outcome status description |
+-----------------------+---------+----------------------------+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| transactionId         | String  | Unique transaction         |
|                       |         | identifier                 |
+-----------------------+---------+----------------------------+
| mnoTransactionId      | String  | Mobile network operator’s  |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+

Example:

Transaction in error:

**Response**

No response body is required. HTTP code 200 is sufficient to acknowledge
the request.

checkStatusC2B 
~~~~~~~~~~~~~~

HTTPS POST request sent from partner’s system to UbiqPay. This request
can be used in case the callback notification is not received due to any
technical issue.

**Path**

/momo/statusc2b

**Request**

===================== ====== =======================================
name                  type   description
===================== ====== =======================================
externalTransactionId String Partner’s unique transaction identifier
===================== ====== =======================================

Example:

**Response**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| status                | String  | Transaction status label   |
|                       |         | (INITIATING, INIT_SUCCESS, |
|                       |         | INIT_UNKNOWN, INIT_ERROR,  |
|                       |         | SUCCESSFUL, ERROR,         |
|                       |         | UNKNOWN)                   |
+-----------------------+---------+----------------------------+
| message               | String  | Outcome status description |
+-----------------------+---------+----------------------------+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| transactionId         | String  | Unique transaction         |
|                       |         | identifier                 |
+-----------------------+---------+----------------------------+
| mnoTransactionId      | String  | Mobile network operator’s  |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+

Example:

Business to customer
--------------------

B2C transaction requests are sent to the API for initiating the process
of customer’s account crediting. Mobile Money account verifications are
performed such as account status, limits check etc. Following successful
checks, the callback outcome request is received.

In case of technical challenges transactions can remain in pending state
for longer periods and are subject to manual reconciliation between
UbiqPay and MNO.

The callbacks with transaction outcomes are directly forwarded to
partner’s system. If partner doesn’t respond or responds with HTTP code
different than 200, a retry mechanism will be activated. This mechanism
is repeating the same callback request until proper response is received
or until it reaches time limit of two days. The retries are being
executed periodically with the laps of exponentially increasing number
of 5 seconds. Meaning, first retry will be initiated 5 seconds after the
final outcome of the transaction is known, second retry will be sent 25
seconds after the previous retry, third retry will be sent 125 seconds
after the previous one and so on.

The interaction between the partner, the UbiqPay system, the MNO system
and the customer is presented on the below diagram.

.. image:: vertopal_44c9d78e0d674f7497f174c873b751ac/media/image3.png
   :width: 6.05556in
   :height: 4.28472in

requestB2C 
~~~~~~~~~~

HTTPS POST request sent from partner’s system to UbiqPay in order to
credit customer’s account.

**Path**

/momo/b2c

**Request**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| confirmB2CUrl         | String  | Partner’s URL for          |
|                       |         | receiving transaction      |
|                       |         | outcome callback           |
|                       |         | notification               |
+-----------------------+---------+----------------------------+
| extra                 | String  | Additional information     |
|                       |         | (description, email        |
|                       |         | address, account ID, …)    |
+-----------------------+---------+----------------------------+

Example:

**Response**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| status                | String  | Transaction status label   |
|                       |         | (INITIATING, INIT_SUCCESS, |
|                       |         | INIT_UNKNOWN, INIT_ERROR,  |
|                       |         | SUCCESSFUL, ERROR,         |
|                       |         | UNKNOWN)                   |
+-----------------------+---------+----------------------------+
| message               | String  | Acknowledgment status      |
|                       |         | description                |
+-----------------------+---------+----------------------------+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| transactionId         | String  | Unique transaction         |
|                       |         | identifier                 |
+-----------------------+---------+----------------------------+

Example:

Transaction in error:

For the list of supported error codes, see Annex, section 1.

confirmB2C 
~~~~~~~~~~

HTTPS POST request sent from UbiqPay system to the partner in order to
confirm the outcome of the transaction.

This request will be retried according to the previously described retry
mechanism.

**Path**

To be determined by partner.

**Request**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| status                | String  | Transaction status label   |
|                       |         | (INITIATING, INIT_SUCCESS, |
|                       |         | INIT_UNKNOWN, INIT_ERROR,  |
|                       |         | SUCCESSFUL, ERROR,         |
|                       |         | UNKNOWN)                   |
+-----------------------+---------+----------------------------+
| message               | String  | Outcome status description |
+-----------------------+---------+----------------------------+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| transactionId         | String  | Unique transaction         |
|                       |         | identifier                 |
+-----------------------+---------+----------------------------+
| mnoTransactionId      | String  | Mobile network operator’s  |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+

Example:

**Response**

No response body is required. HTTP code 200 is sufficient to acknowledge
the request.

checkStatusB2C
~~~~~~~~~~~~~~

HTTPS POST request sent from partner’s system to UbiqPay. This request
can be used in case the callback notification is not received due to any
technical issue.

**Path**

/momo/statusb2c

**Request**

===================== ====== =======================================
name                  type   description
===================== ====== =======================================
externalTransactionId String Partner’s unique transaction identifier
===================== ====== =======================================

Example:

**Response**

+-----------------------+---------+----------------------------+
| name                  | type    | description                |
+=======================+=========+============================+
| status                | String  | Transaction status label   |
|                       |         | (INITIATING, INIT_SUCCESS, |
|                       |         | INIT_UNKNOWN, INIT_ERROR,  |
|                       |         | SUCCESSFUL, ERROR,         |
|                       |         | UNKNOWN)                   |
+-----------------------+---------+----------------------------+
| message               | String  | Outcome status description |
+-----------------------+---------+----------------------------+
| msisdn                | String  | Customer’s mobile phone    |
|                       |         | number in international    |
|                       |         | format                     |
+-----------------------+---------+----------------------------+
| amount                | Decimal | Transaction amount         |
+-----------------------+---------+----------------------------+
| mno                   | String  | Mobile network operator    |
|                       |         | label. One of the          |
|                       |         | following values: ORANGE,  |
|                       |         | VODACOM, AIRTEL.           |
+-----------------------+---------+----------------------------+
| externalTransactionId | String  | Partner’s unique           |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+
| currency              | String  | Currency label in 3-digit  |
|                       |         | ISO format. One of the     |
|                       |         | following values: CDF,     |
|                       |         | USD.                       |
+-----------------------+---------+----------------------------+
| transactionId         | String  | Unique transaction         |
|                       |         | identifier                 |
+-----------------------+---------+----------------------------+
| mnoTransactionId      | String  | Mobile network operator’s  |
|                       |         | transaction identifier     |
+-----------------------+---------+----------------------------+

Example:

Annex

Available error codes
=====================

+------+--------------------------------------------------------------+
| Code | Description                                                  |
+======+==============================================================+
| 4000 | General error                                                |
+------+--------------------------------------------------------------+
| 4001 | The balance is insufficient for the transaction              |
+------+--------------------------------------------------------------+
| 4002 | Request cancelled by user                                    |
+------+--------------------------------------------------------------+
| 4003 | Unable to contact Wallet company. Try again later.           |
+------+--------------------------------------------------------------+
| 4004 | The transaction timed out. That means the customer did not   |
|      | react to the transaction in time                             |
+------+--------------------------------------------------------------+
| 4005 | The given phone number belongs to a carrier that’s currently |
|      | not supported                                                |
+------+--------------------------------------------------------------+
| 4006 | This payment service is temporarily unavailable. Try again   |
|      | later                                                        |
+------+--------------------------------------------------------------+
| 4007 | Declined due to account rule. The account status does not    |
|      | allow this transaction                                       |
+------+--------------------------------------------------------------+


