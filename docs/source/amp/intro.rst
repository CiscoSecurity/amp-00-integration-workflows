Getting Started
===============

`Cisco AMP for Endpoints API Docs <https://api-docs.amp.cisco.com/>`_

A list of AMP scripts can be found `here <https://github.com/topics/amp-for-endpoints>`_.

Region Endpoints
----------------

The following endpoints are available and it is recommended to use v1 for every endpoint.

- `api.amp.cisco.com <https://api-docs.amp.cisco.com/api_resources?api_host=api.amp.cisco.com&api_version=v1>`_
- `api.apjc.amp.cisco.com <https://api-docs.amp.cisco.com/api_resources?api_host=api.apjc.amp.cisco.com&api_version=v1>`_
- `api.eu.amp.cisco.com <https://api-docs.amp.cisco.com/api_resources?api_host=api.eu.amp.cisco.com&api_version=v1>`_


Creating an API Client
----------------------

1. Login to your `AMP for Endpoints Console <https://console.amp.cisco.com/>`_.
2. Go to Accounts then Business Settings.
3. Go to Features and click Configure API Credentials this will generate the ``Client ID`` and a secure ``API Key``.
4. You can now use your credentials to make API calls in the following format:

.. code::

    https://<your_client_id>:<your_api_key>@<api_endpoint>

.. NOTE::
    Alternatively you can use Basic HTTP Authentication. Base 64 encode the string ":", and send that prefixed with the
    string "Basic" as the authorization header. For instance, if your ``client_id`` was 1234, and your ``api_key`` was "atest",
    then it would be base64 encoded to "MTIzNDphdGVzdA==", and an example with your header would be:

    .. http:example::

        POST https://api.amp.cisco.com/v1/event_streams HTTP/1.1
        Authorization: Basic MTIzNDphdGVzdA==

    For more information see `RFC 1945 <http://tools.ietf.org/html/rfc1945#section-11.1>`_. Without proper HTTP Basic
    auth, the API will respond with an error as follows:

    .. code-block:: JSON

        {
        "version":"v1.0.0",
        "data":{},
        "errors":[{
           "error_code":401,
          "description":"Unauthorized",
          "details":["Unknown API key or Client ID"]
         }]
        }


Testing Clients
---------------

To test that a client was created successfully run the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fversion&api_host=api.amp.cisco.com&api_resource=Version&api_version=v1>`_.

.. code::

    GET /v1/version


Integration Requirements
------------------------

General Requirements
^^^^^^^^^^^^^^^^^^^^

- Ability for user to enter the appropriate AMP FQDN.
- Ability for user to enter the API credentials.
- Ability to test credentials and indicate to the user that the integration is able to communicate properly from within the configuration dialog or page.
- An AMQP client used to receive events from the streaming API is preferred.
- The integration should combine Threat Quarantine and Threat Detected events based on the `1.data[].detection_id`` field. The value is returned as a string.
- Ability to link back to AMP for Endpoints console.

Using AMQP Client
^^^^^^^^^^^^^^^^^

Event Stream Management Requirements
""""""""""""""""""""""""""""""""""""

- Ability to easily (one click) create a new event stream with all events and all groups.
- Ability to create an event stream and specify which event types by name and which event groups by name they would like included.
- Ability to list existing Event Streams and their associated event types and groups.
- Ability to delete existing Event Streams.

Requirements That Are Critical For Multinational Customers Who Have Deployments in Multiple AMP Clouds, Customers That Have More Than One Private Cloud Appliance, and MSSPs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Ability to configure more than one event stream as a data source.
- Ability to enter Event Stream URL and credentials independent of the AMP API credentials or any event streams that may be listed.

Rate Limiting
-------------

API Clients are allowed to make a limited number of requests every hour. Each API response will include HTTP headers
detailing the status of their rate limit. If the limit is overrun, then an HTTP 429 Error will be returned.

    - `X-Rate-Limit-Limit` - Total allowed requests in the current period.
    - `X-Rate-Limit-Remaining` - Requests left.
    - `X-Rate-Limit-Reset` - Number of seconds before the limit is reset.

Test Validation
---------------

Test validation is via WebEx. We will go through the integration and walk through the various components and how they
function. We’ll generate various events and observe them being parsed by the product. As part of this we will also
generate 1000+ events and validate how the events are handled. We’ve had issues with AMQP integrations in the past dropping
connections and not recovering resulting in events being dropped. As part of this we’ll need to also require a week
long test where a set number of events are generated throughout the week and validated that all events were captured and
that the AMQP client is still receiving events.




