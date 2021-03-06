.. _api-email-validation:

Email Validation
================

This API endpoint is an email address validation service. Given an arbitrary address, we will validate the address based on:

- Syntax checks (RFC defined grammar)

- DNS validation

- Spell checks

- Email Service Provider (ESP) specific local-part grammar (if available).

Mailgun's email validation service is available for free to all Mailgun customers. It is intended to validate email addresses submitted through forms like newsletters, online registrations and shopping carts.  It is not intended to be used for bulk email list scrubbing and we reserve the right to disable your account if we see it being used as such.

.. note:: No addresses submitted to this service are ever stored on any Mailgun servers. The parser runs completely in memory and no address is persisted after the request is complete.

.. warning:: Do not use your Mailgun private API key. Instead, use your Mailgun public key, available in the My Account tab of the Control Panel. 

.. code-block:: url

     GET /address/validate

Given an arbitrary address, validates address based off defined checks.

.. container:: ptable

 ================= ========================================================
 Parameter         Description
 ================= ========================================================
 address           An email address to validate. (Maximum: 512 characters)
 api_key           If you can not use HTTP Basic Authentication (preferred),
                   you can pass your api_key in as a parameter.
 ================= ========================================================

.. code-block:: url

    GET /address/parse

Parses a delimiter separated list of email addresses into two lists: parsed addresses and unparsable portions. The parsed addresses are a list of addresses that are syntactically valid (and optionally have DNS and ESP specific grammar checks) the unparsable list is a list of characters sequences that the parser was not able to understand. These often align with invalid email addresses, but not always. Delimiter characters are comma (,) and semicolon (;).

.. container:: ptable

 ================= ========================================================
 Parameter         Description
 ================= ========================================================
 addresses         A delimiter separated list of addresses. 
                   (Maximum: 8000 characters)
 syntax_only       Perform only syntax checks or DNS and ESP specific
                   validation as well. (true by default)
 api_key           If you can not use HTTP Basic Authentication (preferred),
                   you can pass your api_key in as a parameter.
 ================= ========================================================

Example
~~~~~~

Validate a single email address.

.. include:: samples/get-validate.rst

Sample response:

.. code-block:: javascript

	{
	    "is_valid": true,
	    "address": "foo@mailgun.net",
	    "parts": {
	        "display_name": ""
	        "local_part": "foo",
	        "domain": "mailgun.net",
	    },
	    "did_you_mean": null
    }

Parse a list of email addresses.

.. include:: samples/get-parse.rst

Sample response:

.. code-block:: javascript

	{
        "parsed": [
            "Alice <alice@example.com>",
            "bob@example.com"
        ],
        "unparseable": [
            "example.com"
        ]
    }
