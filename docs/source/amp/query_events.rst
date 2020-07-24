Query Events
============

Trajectory Events
-----------------

.. NOTE::

    Cisco AMP for Endpoints uses the term trajectory to describe the execution timeline of activities on a
    computer (multi-file execution).

For example, if Computer A was under investigation and you wanted to review the SHA-256s of processes and files on the
computer you would follow these steps:

1. First, query the `Computer Activity <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fcomputers%2F%7B%3Aconnector_guid%7D%2Ftrajectory&api_host=api.amp.cisco.com&api_resource=Computer&api_version=v1>`_ endpoint:

    .. code::

        GET /v1/computers/{:connector_guid}/trajectory

    An example request:

    .. code::

        GET /v1/computers/d821e2d9-9280-489c-a6c3-be02d85ba8a0/trajectory

    An example response:

    .. code-block:: JSON

        {
          "version": "v1.2.0",
          "metadata": {
            "links": {
              "self": "https://api.amp.cisco.com/v1/computers/d821e2d9-9280-489c-a6c3-be02d85ba8a0/trajectory"
            }
          },
          "data": {
            "computer": {
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
            },
            "events": [
              {
                "timestamp": 1581940174,
                "timestamp_nanoseconds": 842009819,
                "date": "2020-02-17T11:49:34+00:00",
                "event_type": "Executed by",
                "group_guids": [
                  "68665863-74d5-4bc1-ac7f-5477b2b6406e"
                ],
                "file": {
                  "disposition": "Clean",
                  "file_name": "taskmgr.exe",
                  "file_path": "/c:/windows/system32/taskmgr.exe",
                  "file_type": "PE Executable",
                  "identity": {
                    "sha256": "292106dfdfacdc0ab33e3cb580ae23f0506cb2402b9b3ca2811a0a1c2f6ebf6c"
                  },
                  "parent": {
                    "disposition": "Clean",
                    "identity": {
                      "sha256": "438b6ccd84f4dd32d9684ed7d58fd7d1e5a75fe3f3d12ab6c788e6bb0ffad5e7"
                    }
                  }
                }
              },
              {
                "timestamp": 1581940173,
                "timestamp_nanoseconds": 543023082,
                "date": "2020-02-17T11:49:33+00:00",
                "event_type": "Executed by",
                "group_guids": [
                  "68665863-74d5-4bc1-ac7f-5477b2b6406e"
                ],
                "file": {
                  "disposition": "Clean",
                  "file_name": "taskmgr.exe",
                  "file_path": "/c:/windows/system32/taskmgr.exe",
                  "file_type": "PE Executable",
                  "identity": {
                    "sha256": "292106dfdfacdc0ab33e3cb580ae23f0506cb2402b9b3ca2811a0a1c2f6ebf6c"
                  },
                  "parent": {
                    "disposition": "Unknown",
                    "identity": {
                      "sha256": "0bd0a04d7b32648f627387894a165b321ac277bd8103a4ca6790607458adf778"
                    }
                  }
                }
              }
            ]
          }
        }


2. Next, you would parse the response for ``.data.events[].file.identity.sha256`` and ``.data.events[].file.parent.identity.sha256``.
3. You would then evaluate these hashes with your product or a 3rd party observable service.

Detection Events
----------------

To hunt for computers that have seen a SHA-256 but have not created an event for that SHA-256 follow these steps:

1.First, query the `Computer Activity <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fcomputers%2Factivity&api_host=api.amp.cisco.com&api_resource=Computer+Activity&api_version=v1>`_ to get a list of computers that have seen the SHA-256 observable:

    .. code::

        GET /v1/computers/activity

    An example request:

    .. code::

        GET /v1/computers/activity?q=814a37d89a79aa3975308e723bc1a3a67360323b7e3584de00896fe7c59bbb8e&offset=0&limit=5

    An example response:

    .. code-block:: JSON

        {
          "version": "v1.2.0",
          "metadata": {
            "links": {
              "self": "https://api.amp.cisco.com/v1/computers/activity?q=814a37d89a79aa3975308e723bc1a3a67360323b7e3584de00896fe7c59bbb8e&offset=0&limit=5"
            },
            "results": {
              "total": 1,
              "current_item_count": 1,
              "index": 0,
              "items_per_page": 5
            }
          },
          "data": [
            {
              "connector_guid": "367a2c23-d0e7-464b-ac3f-9a209868b31d",
              "hostname": "Demo_Stabuniq",
              "windows_processor_id": "83f976a0db415e2",
              "active": true,
              "links": {
                "computer": "https://api.amp.cisco.com/v1/computers/367a2c23-d0e7-464b-ac3f-9a209868b31d",
                "trajectory": "https://api.amp.cisco.com/v1/computers/367a2c23-d0e7-464b-ac3f-9a209868b31d/trajectory?q=814a37d89a79aa3975308e723bc1a3a67360323b7e3584de00896fe7c59bbb8e",
                "group": "https://api.amp.cisco.com/v1/groups/b077d6bc-bbdf-42f7-8838-a06053fbd98a"
              }
            }
          ]
        }

2. Store the values of ``.data[].connector_guid``.
3. Query the `Events Endpoint <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fevents&api_host=api.amp.cisco.com&api_resource=Event&api_version=v1>`_ to see what events were generated for the same SHA-256 observable:

    .. code::

        GET /v1/events

    An example request:

    .. code::

        GET /v1/events?detection_sha256=b630e72639cc7340620adb0cfc26332ec52fe8867b769695f2d25718d68b1b40&limit=1

    An example response:

    .. code-block:: JSON

        {
          "version": "v1.2.0",
          "metadata": {
            "links": {
              "self": "https://api.amp.cisco.com/v1/events?detection_sha256=b630e72639cc7340620adb0cfc26332ec52fe8867b769695f2d25718d68b1b40&limit=1",
              "next": "https://api.amp.cisco.com/v1/events?detection_sha256=b630e72639cc7340620adb0cfc26332ec52fe8867b769695f2d25718d68b1b40&limit=1&offset=1"
            },
            "results": {
              "total": 4,
              "current_item_count": 1,
              "index": 0,
              "items_per_page": 1
            }
          },
          "data": [
            {
              "id": 6180352115244794000,
              "timestamp": 1582222838,
              "timestamp_nanoseconds": 279000000,
              "date": "2020-02-20T18:20:38+00:00",
              "event_type": "Threat Detected",
              "event_type_id": 1090519054,
              "detection": "W32.GenericKD:ZVETJ.18gs.1201",
              "detection_id": "6180352115244793858",
              "connector_guid": "20a0ce9f-44d1-4cbb-ab04-8a0705448b72",
              "group_guids": [
                "6c3c2005-4c74-4ba7-8dbb-c4d5b6bafe03"
              ],
              "severity": "Medium",
              "computer": {
                "connector_guid": "20a0ce9f-44d1-4cbb-ab04-8a0705448b72",
                "hostname": "Demo_Upatre",
                "external_ip": "69.226.122.127",
                "user": "A@TEMPLATE-W7X86",
                "active": true,
                "network_addresses": [
                  {
                    "ip": "230.122.135.241",
                    "mac": "3f:1e:b2:28:25:24"
                  }
                ],
                "links": {
                  "computer": "https://api.amp.cisco.com/v1/computers/20a0ce9f-44d1-4cbb-ab04-8a0705448b72",
                  "trajectory": "https://api.amp.cisco.com/v1/computers/20a0ce9f-44d1-4cbb-ab04-8a0705448b72/trajectory",
                  "group": "https://api.amp.cisco.com/v1/groups/6c3c2005-4c74-4ba7-8dbb-c4d5b6bafe03"
                }
              },
              "file": {
                "disposition": "Malicious",
                "file_name": "wsymqyv90.exe",
                "file_path": "\\\\?\\C:\\Users\\Administrator\\AppData\\Local\\Temp\\OUTLOOK_TEMP\\wsymqyv90.exe",
                "identity": {
                  "sha256": "b630e72639cc7340620adb0cfc26332ec52fe8867b769695f2d25718d68b1b40",
                  "sha1": "70aef829bec17195e6c8ec0e6cba0ed39f97ba48",
                  "md5": "e2f5dcd966e26d54329e8d79c7201652"
                },
                "parent": {
                  "process_id": 4040,
                  "disposition": "Clean",
                  "file_name": "iexplore.exe",
                  "identity": {
                    "sha256": "b4e5c2775de098946b4e11aba138b89d42b88c1dbd4d5ec879ef6919bf018132",
                    "sha1": "8de30174cebc8732f1ba961e7d93fe5549495a80",
                    "md5": "b3581f426dc500a51091cdd5bacf0454"
                  }
                }
              }
            }
          ]
        }

4. Store the values of ``.data[].connector_guid``.
5. Diff the ``connector_guid`` values found in step two with the ``connector_guid`` values found in step four.
6. Create a high priority alert for the endpoints that have seen the file but did not generate any events for it. This means that there is malicious activity that needs to be sent as an alert to the SOC.

.. NOTE::

    Example implementations of similar workflows can be found here:

    - https://github.com/CiscoSecurity/amp-04-sha256-to-command-line-arguments
    - https://github.com/CiscoSecurity/amp-04-check-sha256-execution
    - https://github.com/CiscoSecurity/amp-04-sha256-to-network-connections

