MobileMoney
===========

There are two types of Mobile Money transactions: customer to business
(C2B) and business to customer (B2C). Both transactions are two-step
based. Just before initiating on Mobile Network Operator (MNO) it is
being marked as *initiating*. Depending on the MNO response the
transaction will switch to *init_success*, *init_unknown* or
*init_error* state. The *init_unknown* state means that the initiation
outcome is unknown and transaction may or may not be successfully
initiated. The init_error state means there was a definite error and the
transaction could not have been initiated.

Following the callback notification from the MNO, the transaction is
moved to either *successful*, *error* or *unknown* state. The
transactions in *unknown* state will move to *successful* or *error*
after the reconciliation.

The transaction lifecycle is presented on the below graph.

.. image:: vertopal_44c9d78e0d674f7497f174c873b751ac/media/image1.png
   :width: 4.77011in
   :height: 3.1in
