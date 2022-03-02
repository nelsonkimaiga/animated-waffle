Overview
========

Current document is presenting the APIs for Mobile Money and SMS
interaction.

Mobile Money APIs allow customer’s account debiting and crediting
operations. For each operation there is a callback notification to
partner’s system that is providing the final status of the transaction.
Additionally, it is possible to verify the status for each Mobile Money
transaction in case of uncertainty due to any technical issue.

SMS APIs allow dispatching by specifying customer’s phone number, sender
name and the message itself. For every SMS being sent there are delivery
callback notifications to partner’s system.

For both types of API callback requests there is a retry mechanism that
gets activated if no acknowledgment is received to the initial request.

APIs are formatted according to REST/JSON protocol specification. All of
the communication with partner’s system is managed through the HTTPS
connection. Incoming traffic is accepted only for whitelisted set of IP
addresses.
