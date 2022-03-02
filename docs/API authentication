API authentication
------------------

Authentication of requests is managed through HTTP headers containing
following parameters:

-  AccountId - parameter is shared during the integration process
   together with the account password which will allow for accessToken
   generation.

-  Timestamp - a time-based value respecting the following format:
   YYYYMMDDHHmmss.

-  AccessToken - represents the SHA-256 hashed value of the concatenated
   strings: accountId, password and timestamp.

These parameters are to be specified for every incoming request.
