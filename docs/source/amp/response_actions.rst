Response Actions
================

Simple Custom Detections
------------------------

A best practice for organizations is to only have one Simple Custom Detection (SCD) list and to use that SCD list in all
policies. A policy can only have one SCD list configured at a time. A SHA256 added to an SCD list will not be alerted on
or quarantined, if it is seen on a computer that does not have that `SCD list <https://console.amp.cisco.com/custom_detections/simple>`_
applied to the `policy <https://console.amp.cisco.com/policies>`_ that is applied to the `group <https://console.amp.cisco.com/groups>`_
the computer is in.

Get SCD List GUIDs
^^^^^^^^^^^^^^^^^^

To add a SHA256 to a SCD query using the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Ffile_lists%2Fsimple_custom_detections&api_host=api.amp.cisco.com&api_resource=File+List&api_version=v1>`_
to get a list of the SCD file lists that are available:

.. code::

    GET /v1/file_lists/simple_custom_detections

This will return a list of SCD file lists for the organization:

.. code-block:: JSON

    {
      "version": "v1.2.0",
      "metadata": {
        "links": {
          "self": "https://api.amp.cisco.com/v1/file_lists/simple_custom_detections?limit=3&offset=2",
          "prev": "https://api.amp.cisco.com/v1/file_lists/simple_custom_detections?limit=3&offset=0",
          "next": "https://api.amp.cisco.com/v1/file_lists/simple_custom_detections?limit=3&offset=5"
        },
        "results": {
          "total": 7,
          "current_item_count": 3,
          "index": 2,
          "items_per_page": 3
        }
      },
      "data": [
        {
          "name": "Sample SCD List 1",
          "guid": "e773a9eb-296c-40df-98d8-bed46322589d",
          "type": "simple_custom_detections",
          "links": {
            "file_list": "https://api.amp.cisco.com/v1/file_lists/021f6434-0b67-4790-8601-b535d66ca0fb"
          }
        },
        {
          "name": "Sample SCD List 2",
          "guid": "db2b9dd6-94d2-4acc-a6cb-c4c66c9199a1",
          "type": "simple_custom_detections",
          "links": {
            "file_list": "https://api.amp.cisco.com/v1/file_lists/db2b9dd6-94d2-4acc-a6cb-c4c66c9199a1"
          }
        }
      ]
    }

From the response parse out ``.data[].guid`` and ``.data[].name``. Present the user with the list of names found in
``.data[].name``. When the user selects the SCD list they would like to add or remove a SHA256 to save the
``.data[].guid`` for that SCD list.

Add a SHA256 to a SCD List
^^^^^^^^^^^^^^^^^^^^^^^^^^

To add a SHA256 to a SCD list use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=POST+%2Fv1%2Ffile_lists%2F%7B%3Afile_list_guid%7D%2Ffiles%2F%7B%3Asha256%7D&api_host=api.amp.cisco.com&api_resource=File+List+Item&api_version=v1>`_:

.. code::

    POST /v1/file_lists/{:file_list_guid}/files/{:sha256}

If the user chose `Sample SCD List 1` and wanted to add the SHA256 ``d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b`` the request would be:

.. code::

    POST /v1/file_lists/e773a9eb-296c-40df-98d8-bed46322589d/files/d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b

Optionally a description can be send in the request body:

.. code-block:: JSON

    {"description":"Added from Product XYZ as part of Incident 123"}

A successful request will return a ``201`` status with a response body like this:

.. code-block:: JSON

    {
      "version": "v1.2.0",
      "metadata": {
        "links": {
          "self": "https://api.amp.cisco.com/v1/file_lists/e773a9eb-296c-40df-98d8-bed46322589d/files/d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b`"
        }
      },
      "data": {
        "sha256": "d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b",
        "description": "Added from Product XYZ as part of Incident 123",
        "source": "Created by entering SHA-256 via Public api.",
        "links": {
          "file_list": "https://api.amp.cisco.com/v1/file_lists/e773a9eb-296c-40df-98d8-bed46322589d"
        }
      }
    }

Remove a SHA256 from a SCD List
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To remove a SHA256 from a SCD list use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=DELETE+%2Fv1%2Ffile_lists%2F%7B%3Afile_list_guid%7D%2Ffiles%2F%7B%3Asha256%7D&api_host=api.amp.cisco.com&api_resource=File+List+Item&api_version=v1>`_:

.. code::

    DELETE /v1/file_lists/{:file_list_guid}/files/{:sha256}

If the user chose `Sample SCD List 1` and wanted to remove the SHA256 ``d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b`` the request would be:

.. code::

    DELETE /v1/file_lists/e773a9eb-296c-40df-98d8-bed46322589d/files/d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b

A successful request will return ``200`` status with a response body like this:

.. code-block:: JSON

    {
      "version": "v1.2.0",
      "metadata": {
        "links": {
          "self": "https://api.amp.cisco.com/v1/file_lists/e773a9eb-296c-40df-98d8-bed46322589d/files/d5cb3ef9816e8fd30cc9537bb394a7cc6c46c1dff1c65f11b527ef1df14edc3b"
        }
      },
      "data": {
      }
    }

Application Block List
----------------------

Get Application Block List GUIDs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To add a SHA256 to an Application Block List first query use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Ffile_lists%2Fapplication_blocking&api_host=api.amp.cisco.com&api_resource=File+List&api_version=v1>`_
to get a list of the Application Block Lists that are available:

.. code::

    GET /v1/file_lists/application_blocking

This will return a list of Application Block Lists for the organization:

.. code-block:: JSON

    {
      "version": "v1.2.0",
      "metadata": {
        "links": {
          "self": "https://api.amp.cisco.com/v1/file_lists/application_blocking?limit=3&offset=2",
          "prev": "https://api.amp.cisco.com/v1/file_lists/application_blocking?limit=3&offset=0",
          "next": "https://api.amp.cisco.com/v1/file_lists/application_blocking?limit=3&offset=5"
        },
        "results": {
          "total": 6,
          "current_item_count": 3,
          "index": 2,
          "items_per_page": 3
        }
      },
      "data": [
        {
          "name": "Sample Application Blocking List 2",
          "guid": "e4984c9b-651a-499e-a6fe-9ee938dab661",
          "type": "application_blocking",
          "links": {
            "file_list": "https://api.amp.cisco.com/v1/file_lists/e4984c9b-651a-499e-a6fe-9ee938dab661"
          }
        },
        {
          "name": "Sample Application Blocking List 3",
          "guid": "0fda9022-9491-4982-9066-adc4f65007bc",
          "type": "application_blocking",
          "links": {
            "file_list": "https://api.amp.cisco.com/v1/file_lists/0fda9022-9491-4982-9066-adc4f65007bc"
          }
        }
      ]
    }

From the response parse out ``.data[].guid`` and ``.data[].name``. Present the user with the list of names found in
``.data[].name``. When the user selects the Application Block List they would like to add or remove a SHA256 to save the
``.data[].guid`` for that Application Block List.

Add a SHA256 to an Application Block List
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To add a SHA256 to an Application Block list use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=POST+%2Fv1%2Ffile_lists%2F%7B%3Afile_list_guid%7D%2Ffiles%2F%7B%3Asha256%7D&api_host=api.amp.cisco.com&api_resource=File+List+Item&api_version=v1>`_:

.. code::

    POST /v1/file_lists/{:file_list_guid}/files/{:sha256}


Remove a SHA256 from an Application Block List
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To remove a SHA256 from an Application Block List use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=DELETE+%2Fv1%2Ffile_lists%2F%7B%3Afile_list_guid%7D%2Ffiles%2F%7B%3Asha256%7D&api_host=api.amp.cisco.com&api_resource=File+List+Item&api_version=v1>`_:

.. code::

    DELETE /v1/file_lists/{:file_list_guid}/files/{:sha256}

Move Host to Group
------------------

Get Group GUIDs
^^^^^^^^^^^^^^^

To fetch a list of all groups and their associated GUIDs use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fgroups&api_host=api.amp.cisco.com&api_resource=Group&api_version=v1>`_:

.. code::

    GET /v1/groups

From the response parse out ``.data[].guid`` and ``.data[].name``. Present the user with the list of groups found in
``.data[].name``. When the user selects the group they would like to move hosts to save the
``.data[].guid`` for that group.

Moving a Host to a Group
^^^^^^^^^^^^^^^^^^^^^^^^

To move a computer to a group with a given connector_guid and group_guid use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=PATCH+%2Fv1%2Fcomputers%2F%7B%3Aconnector_guid%7D&api_host=api.amp.cisco.com&api_resource=Computer&api_version=v1>`_:

.. http:example::

    PATCH https://api.amp.cisco.com/v1/computers/{:connector_guid} HTTP/1.1

    {"group_guid": "{:group_guid}"}

An example cURL request:

.. code::

    curl -X PATCH \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'content-length: 53' \
    --compressed -H 'Accept-Encoding: gzip, deflate' \
    -d '{"group_guid":"68665863-74d5-4bc1-ac7f-5477b2b6406e"}' \
    -u YOUR_API_CLIENT_ID \
    'https://api.amp.cisco.com/v1/computers/d821e2d9-9280-489c-a6c3-be02d85ba8a0'

Example response:

.. code-block:: JSON

    {
      "version": "v1.2.0",
      "metadata": {
        "links": {
          "self": "https://api.amp.cisco.com/v1/computers/d821e2d9-9280-489c-a6c3-be02d85ba8a0"
        }
      },
      "data": {
        "connector_guid": "d821e2d9-9280-489c-a6c3-be02d85ba8a0",
        "hostname": "Demo_Command_Line_Arguments_Kovter",
        "windows_processor_id": "1937b8e046adf25",
        "active": true,
        "links": {
          "computer": "https://api.amp.cisco.com/v1/computers/d821e2d9-9280-489c-a6c3-be02d85ba8a0",
          "trajectory": "https://api.amp.cisco.com/v1/computers/d821e2d9-9280-489c-a6c3-be02d85ba8a0/trajectory",
          "group": "https://api.amp.cisco.com/v1/groups/68665863-74d5-4bc1-ac7f-5477b2b6406e"
        },
        "connector_version": "99.0.99.11594",
        "operating_system": "Windows 10, SP 0.0",
        "internal_ips": [
          "48.228.237.163"
        ],
        "external_ip": "87.18.29.150",
        "group_guid": "68665863-74d5-4bc1-ac7f-5477b2b6406e",
        "install_date": "2020-02-17T08:47:17Z",
        "network_addresses": [
          {
            "mac": "cd:e0:30:42:21:f7",
            "ip": "48.228.237.163"
          }
        ],
        "policy": {
          "guid": "75f5a2b7-2875-41c1-9a11-0b212f347a08",
          "name": "Triage Policy"
        },
        "faults": [

        ],
        "isolation": {
          "available": false,
          "status": "not_isolated"
        },
        "orbital": {
          "status": "not_enabled"
        }
      }
    }

Isolate Host
------------

API Workflow: Isolate Host Based on Presence of File Hash
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To isolate hosts based on a SHA256 hash that is present on the system first query the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fcomputers%2Factivity&api_host=api.amp.cisco.com&api_resource=Computer+Activity&api_version=v1>`_
with the ``q`` parameter set to the SHA256 you want to lookup:

.. code::

    GET /v1/computers/activity?q=SHA256

This will return a list of computers that have seen that SHA256 regardless of any AMP for Endpoint event, the response
will return a maximum of 500 endpoints per page. The ``.metadata.next`` URL can be queried to get the next page of
endpoints and will only be present if there is more than one page of results.

.. code-block:: JSON

    {
      "version": "v1.2.0",
      "metadata": {
        "links": {
          "self": "https://api.amp.cisco.com/v1/computers/activity?q=SearchProtocolHost.exe&limit=5",
          "next": "https://api.amp.cisco.com/v1/computers/activity?q=SearchProtocolHost.exe&limit=5&offset=5"
        },
        "results": {
          "total": 10,
          "current_item_count": 5,
          "index": 0,
          "items_per_page": 5
        }
      },
      "data": [
        {
          "connector_guid": "043a414d-5520-4374-b545-dff6a0e74195",
          "hostname": "Demo_CozyDuke",
          "windows_processor_id": "d83597eb420f61a",
          "active": true,
          "links": {
            "computer": "https://api.amp.cisco.com/v1/computers/043a414d-5520-4374-b545-dff6a0e74195",
            "trajectory": "https://api.amp.cisco.com/v1/computers/043a414d-5520-4374-b545-dff6a0e74195/trajectory?q=SearchProtocolHost.exe",
            "group": "https://api.amp.cisco.com/v1/groups/6c3c2005-4c74-4ba7-8dbb-c4d5b6bafe03"
          }
        },
        {
          "connector_guid": "20a0ce9f-44d1-4cbb-ab04-8a0705448b72",
          "hostname": "Demo_Upatre",
          "windows_processor_id": "70bd6284e15af93",
          "active": true,
          "links": {
            "computer": "https://api.amp.cisco.com/v1/computers/20a0ce9f-44d1-4cbb-ab04-8a0705448b72",
            "trajectory": "https://api.amp.cisco.com/v1/computers/20a0ce9f-44d1-4cbb-ab04-8a0705448b72/trajectory?q=SearchProtocolHost.exe",
            "group": "https://api.amp.cisco.com/v1/groups/6c3c2005-4c74-4ba7-8dbb-c4d5b6bafe03"
          }
        }
      ]
    }

From the response parse out the ``.data[].connector_guid`` values and run the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=OPTIONS+%2Fv1%2Fcomputers%2F%7B%3Aconnector_guid%7D%2Fisolation&api_host=api.amp.cisco.com&api_resource=Endpoint+Isolation&api_version=v1>`_
for each connector GUID to validate the endpoint can be put into isolation:

.. code::

    OPTIONS /v1/computers/{:connector_guid}/isolation

An example response from the OPTIONS query:

.. code::

    strict-transport-security: max-age=31536000
    status: 204 No Content
    x-ratelimit-limit: 3000
    x-ratelimit-reset: 3595
    x-ratelimit-remaining: 2982
    x-frame-options: SAMEORIGIN
    allow: OPTIONS, GET, PUT
    x-ratelimit-resetdate: 2020-02-20T19:42:33Z
    transfer-encoding: chunked

The ``allow`` values will show which options are available. If ``PUT`` is available use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=PUT+%2Fv1%2Fcomputers%2F%7B%3Aconnector_guid%7D%2Fisolation&api_host=api.amp.cisco.com&api_resource=Endpoint+Isolation&api_version=v1>`_
to start isolation for that host:

.. code::

    PUT /v1/computers/{:connector_guid}/isolation

If you skip checking what options are available and try to start isolation you will receive an error if the host is not
a supported OS, isolation is not enabled in the policy, or the endpoint is in a transitional state or is already isolated.

Checking an Endpoint for Vulnerable Software
--------------------------------------------

General Organization Collection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

List of all Vulnerabilities in an Organization
""""""""""""""""""""""""""""""""""""""""""""""

For a general query of all vulnerabilities in the organization use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fvulnerabilities&api_host=api.amp.cisco.com&api_resource=Vulnerabilities&api_version=v1>`_:

.. code::

    GET /v1/vulnerabilities

List of Specific Computers Within an Organization That Have Observed a Vulnerability With a Given SHA-256
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

To provide a list of computers on which the vulnerability has been observed with given SHA-256 use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fvulnerabilities%2F%7B%3Asha256%7D%2Fcomputers&api_host=api.amp.cisco.com&api_resource=Vulnerabilities&api_version=v1>`_:

.. code::

    GET /v1/vulnerabilities/{:sha256}/computers

Specific Endpoint Collection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To fetch a list of events from a specific computer that has vulnerabilities use the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fevents&api_host=api.amp.cisco.com&api_resource=Event&api_version=v1>`_:

.. code::

    GET /v1/events?connector_guid[]={:connector_guid}&event_type[]=1107296279


