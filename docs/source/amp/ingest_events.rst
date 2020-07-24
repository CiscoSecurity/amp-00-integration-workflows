Ingest Events
=============

Streaming API (Preferred)
-------------------------

`AMP for Endpoints Streaming API <https://api-docs.amp.cisco.com/api_resources/EventStream?api_host=api.amp.cisco.com&api_version=v1>`_

The AMP for Endpoints Streaming API is used to collect events from AMP for Endpoints. It is based on AMQP 0.9.1 and is implemented using
`Rabbit MQ <https://www.rabbitmq.com/amqp-0-9-1-reference.html>`_. Communication with with the streaming API requires
an AMQP client, a list of clients libraries can be found `here <https://www.rabbitmq.com/devtools.html>`_.

- The passive and durable bits should be set on the queue
- The protocol is over SSL/TLS.

The events in an event stream can be received using a persistent connection or queried on an interval. When using a
persistent connection the client will connect to the stream and wait for events to be generated. When the events are
generated they will be consumed immediately. If queried on an interval the events will sit in the queue waiting to be
consumed. When the client connects it will consume all of the events in the queue and then close the connection. Events
will sit in the queue for 10 days maximum, at which point they will be deleted.

Create Stream
^^^^^^^^^^^^^

Customers must create an Event Stream using the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=POST+%2Fv1%2Fevent_streams&api_host=api.amp.cisco.com&api_resource=EventStream&api_version=v1>`_.

.. code::

    POST /v1/event_streams

They have the options of specifying which `event types <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fevent_types&api_host=api.amp.cisco.com&api_resource=Event+Type&api_version=v1>`_
they would like in the stream as well as which `groups <https://api-docs.amp.cisco.com/api_actions/details?api_action=GET+%2Fv1%2Fgroups&api_host=api.amp.cisco.com&api_resource=Group&api_version=v1>`_
they would like to receive events from. Organizations are limited to a maximum of 5 event streams.

The credentials for an event stream are only provided at time of creation. if they are not stored at this point in time
there is no ability to retrieve or reset the credentials for the stream. The stream will have to be deleted and a new
stream with the same settings will have to be created.

`Example Create Stream Script <https://github.com/CiscoSecurity/amp-04-create-event-stream>`_

`Example Duplicate Stream Script <https://github.com/CiscoSecurity/amp-04-duplicate-event-stream>`_


Connect to Stream
^^^^^^^^^^^^^^^^^

Information on how to use Pika to connect to a stream can be found in the example below.

`Example Stream Consumer <https://github.com/Cisco-AMP/pika_bootstrap>`_

Delete Stream
^^^^^^^^^^^^^
Customers can delete an Event Stream using the following `request <https://api-docs.amp.cisco.com/api_actions/details?api_action=DELETE+%2Fv1%2Fevent_streams%2F%7B%3Aid%7D&api_host=api.amp.cisco.com&api_resource=EventStream&api_version=v1>`_.

.. code::

    DELETE /v1/event_streams/{:id}

`Example Delete Stream Script <https://github.com/CiscoSecurity/amp-04-delete-event-stream>`_

Correlating Events
------------------

There can be three different types of events that can all have the same ``detection_id`` for the same detection. These
events are Threat Detected, Threat Quarantine, and Quarantine Failure events. Similarly, the ``detection_id`` can be the
same for all detections in Cloud Recall Quarantine Successful and Cloud Recall Detection events. The integration should
combine Threat Quarantine and Threat Detected events based on the ``.data[].detection_id`` field.
The value is returned as a string. This makes for a much better user experience as it allows to the user to
differentiate between a quarantined threat that poses a less immediate risk and a threat that may still be on the
machine. Not all products have the capacity to implement this. In AMP for Endpoints a Quarantine event (success or
failure) should be associated with a Threat Detected event. The UI will combine these into a single event making it easy
for users to see which things to focus on. In the API these events are separated. To correlate Threat Detected with
Quarantined actions you have to track ``.data[].detection_id``.

The following event JSON shows the correlation of the ``detection_id`` value of 6533241145273614338:

.. code-block:: JSON


    [
        {
          "id": 6533241145273614340,
          "timestamp": 1593079552,
          "timestamp_nanoseconds": 619000000,
          "date": "2020-06-25T10:05:52+00:00",
          "event_type": "Threat Quarantined",
          "event_type_id": 553648143,
          "detection_id": "6533241145273614338",
          "connector_guid": "0b4883d4-8ecf-4404-9453-13cba0ee7662",
          "group_guids": [
            "d7cf8b98-e830-4ce1-a0e5-d943ed6bab17"
          ],
          "severity": "Medium",
          "computer": {
            "connector_guid": "0b4883d4-8ecf-4404-9453-13cba0ee7662",
            "hostname": "Demo_AMP_Threat_Quarantined",
            "external_ip": "163.32.98.150",
            "active": true,
            "network_addresses": [
              {
                "ip": "50.88.43.2",
                "mac": "87:9c:f8:c6:c9:cf"
              }
            ],
            "links": {
              "computer": "https://api.amp.cisco.com/v1/computers/0b4883d4-8ecf-4404-9453-13cba0ee7662",
              "trajectory": "https://api.amp.cisco.com/v1/computers/0b4883d4-8ecf-4404-9453-13cba0ee7662/trajectory",
              "group": "https://api.amp.cisco.com/v1/groups/d7cf8b98-e830-4ce1-a0e5-d943ed6bab17"
            }
          },
          "file": {
            "disposition": "Malicious",
            "identity": {
              "sha256": "a78c29d1fa05c2b23d1dc9b75da8c053399143682fe3779bc466f10e1a997850"
            }
          }
        },
        {
          "id": 6533241145273614339,
          "timestamp": 1593079552,
          "timestamp_nanoseconds": 619000000,
          "date": "2020-06-25T10:05:52+00:00",
          "event_type": "Threat Detected",
          "event_type_id": 1090519054,
          "detection_id": "6533241145273614338",
          "connector_guid": "0b4883d4-8ecf-4404-9453-13cba0ee7662",
          "group_guids": [
            "d7cf8b98-e830-4ce1-a0e5-d943ed6bab17"
          ],
          "severity": "Medium",
          "computer": {
            "connector_guid": "0b4883d4-8ecf-4404-9453-13cba0ee7662",
            "hostname": "Demo_AMP_Threat_Quarantined",
            "external_ip": "163.32.98.150",
            "active": true,
            "network_addresses": [
              {
                "ip": "50.88.43.2",
                "mac": "87:9c:f8:c6:c9:cf"
              }
            ],
            "links": {
              "computer": "https://api.amp.cisco.com/v1/computers/0b4883d4-8ecf-4404-9453-13cba0ee7662",
              "trajectory": "https://api.amp.cisco.com/v1/computers/0b4883d4-8ecf-4404-9453-13cba0ee7662/trajectory",
              "group": "https://api.amp.cisco.com/v1/groups/d7cf8b98-e830-4ce1-a0e5-d943ed6bab17"
            },
            "user": "johndoe"
          },
          "file": {
            "disposition": "Malicious",
            "identity": {
              "sha256": "a78c29d1fa05c2b23d1dc9b75da8c053399143682fe3779bc466f10e1a997850",
              "sha1": "cf162622e29bca072d01b274fbbc3ceaacdd13c7",
              "md5": "0fe5be3811a98ee6a9c997d3812d911a"
            },
            "file_name": "SqGGuYXyy.exe",
            "file_path": "\\\\?\\C:\\SqGGuYXyy.exe",
            "parent": {
              "process_id": 896,
              "disposition": "Clean",
              "file_name": "svchost.exe",
              "identity": {
                "sha256": "121118a0f5e0e8c933efd28c9901e54e42792619a8a3a6d11e1f0025a7324bc2",
                "sha1": "4af001b3c3816b860660cf2de2c0fd3c1dfb4878",
                "md5": "54a47f6b5e09a77e61649109c6a08866"
              }
            }
          },
          "detection": "W32.Overdrive.RET"
        }
    ]


Grouping Events
---------------

Same Detection (Hash) on Multiple Endpoints in N Time Period
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Assuming N time period is 5 minutes the following "events" would be in one incident:

.. code-block:: JSON

    {"sha256":"8ed3f6ad685b959ead7022518e1af76cd816f8e8ec7ccdda1ed4018e8f2223f8", "date":"2019-09-19T18:00:00+00:00", "computer":"alpha"}
    {"sha256":"8ed3f6ad685b959ead7022518e1af76cd816f8e8ec7ccdda1ed4018e8f2223f8", "date":"2019-09-19T18:00:23+00:00", "computer":"bravo"}
    {"sha256":"8ed3f6ad685b959ead7022518e1af76cd816f8e8ec7ccdda1ed4018e8f2223f8", "date":"2019-09-19T18:02:47+00:00", "computer":"charlie"}
    {"sha256":"8ed3f6ad685b959ead7022518e1af76cd816f8e8ec7ccdda1ed4018e8f2223f8", "date":"2019-09-19T18:03:51+00:00", "computer":"delta"}
    {"sha256":"8ed3f6ad685b959ead7022518e1af76cd816f8e8ec7ccdda1ed4018e8f2223f8", "date":"2019-09-19T18:04:44+00:00", "computer":"echo"}

The next "event" would go to a new incident:

.. code-block:: JSON

    {"sha256":"8ed3f6ad685b959ead7022518e1af76cd816f8e8ec7ccdda1ed4018e8f2223f8", "date":"2019-09-19T18:09:13+00:00", "computer":"foxtrot"}

Same Detection (Hash) on Multiple Endpoints on Same Endpoint in N Time Period
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Assuming N time period is 5 minutes the following "events" would be in one incident:

.. code-block:: JSON

    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:00:00+00:00", "computer":"golf"}
    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:00:23+00:00", "computer":"golf"}
    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:02:47+00:00", "computer":"golf"}
    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:03:51+00:00", "computer":"golf"}
    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:04:44+00:00", "computer":"golf"}

The next "event" would go to a new incident:

.. code-block:: JSON

    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:09:13+00:00", "computer":"golf"}

Multiple Detections (Hash or IP) on the Same Computer in N Time Period
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Assuming N time period is 5 minutes the following "events" would be in one incident:

.. code-block:: JSON

    {"sha256":"b9dd960c1753459a78115d3cb845a57d924b6877e805b08bd01086ccdf34433c", "date":"2019-09-19T18:00:00+00:00", "computer":"hotel"}
    {"sha256":"4f4a9410ffcdf895c4adb880659e9b5c0dd1f23a30790684340b3eaacb045398", "date":"2019-09-19T18:00:23+00:00", "computer":"hotel"}
    {"sha256":"092c79e8f80e559e404bcf660c48f3522b67aba9ff1484b0367e1a4ddef7431d", "date":"2019-09-19T18:02:47+00:00", "computer":"hotel"}
    {"black_list_ip":"1.2.3.4", "date":"2019-09-19T18:03:51+00:00", "computer":"hotel"}
    {"black_list_ip":"4.3.2.1", "date":"2019-09-19T18:04:44+00:00", "computer":"hotel"}

The next "event" would go to a new incident:

.. code-block:: JSON

    {"sha256":"f144a6907dc4284d1f9fe6a7d9b9ff53c02c1d07ba68f24d413d7ff7f757a782", "date":"2019-09-19T18:09:13+00:00", "computer":"hotel"}

