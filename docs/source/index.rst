Welcome to UbiqPay's documentation!
===================================

=================
API specification
=================

+---------+------------+---------------------------------------------+
| Version | Date       | Update                                      |
+=========+============+=============================================+
| 1.0.0   | 2020-02-19 | Initial version                             |
+---------+------------+---------------------------------------------+
| 1.0.1   | 2020-03-09 | Updated paths, Mobile Money transaction     |
|         |            | statuses and state chart                    |
+---------+------------+---------------------------------------------+
| 1.1.0   | 2021-08-23 | Add field “extra” in C2B and B2C            |
+---------+------------+---------------------------------------------+
| 1.2.0   | 2021-10-25 | Add field “paymentInstructions” in c2b      |
|         |            | response                                    |
+---------+------------+---------------------------------------------+
| 1.2.1   | 2021-11-10 | Add examples of transactions in error and   |
|         |            | available error codes                       |
+---------+------------+---------------------------------------------+

.. note::

   This project is under active development.

Contents
========

.. toctree::

   Overview
   API authentication
   Transactions reconciliation
   MobileMoney
   Customer to business (C2B)
   requestC2B
   confirmC2B
   checkStatusC2B
   Business to customer
   requestB2C
   confirmB2C
   checkStatusB2C
   SMS
   sendSMS
   receiveDLR
   checkStatusSMS

