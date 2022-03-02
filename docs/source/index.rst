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

`1 Overview 1 <#overview>`__

`1.1 API authentication 1 <#api-authentication>`__

`1.2 Transactions reconciliation 1 <#transactions-reconciliation>`__

`2 MobileMoney 2 <#mobilemoney>`__

`2.1 Customer to business (C2B) 2 <#customer-to-business-c2b>`__

`2.1.1 requestC2B 3 <#requestc2b>`__

`2.1.2 confirmC2B 5 <#confirmc2b>`__

`2.1.3 checkStatusC2B 6 <#checkstatusc2b>`__

`2.2 Business to customer 8 <#business-to-customer>`__

`2.2.1 requestB2C 9 <#requestb2c>`__

`2.2.2 confirmB2C 11 <#confirmb2c>`__

`2.2.3 checkStatusB2C 12 <#checkstatusb2c>`__

`3 SMS 14 <#_Toc33046076>`__

`3.1 sendSMS 14 <#_Toc33046077>`__

`3.2 receiveDLR 16 <#_Toc33046078>`__

`3.3 checkStatusSMS 17 <#_Toc33046079>`__

