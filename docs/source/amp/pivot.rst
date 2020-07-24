Pivot into AMP for Endpoints
============================

Pivoting by Search
------------------

Search File Trajectory, Device Trajectory, File Analysis, Users, Groups, Policies, and other sources using th following URL format:

.. code::

    https://console.amp.cisco.com/search?query=<QUERY>

Example with Computer Name:

.. code::

    https://console.amp.cisco.com/search?query=Demo_AMP

Example with SHA-256:

.. code::

    https://console.amp.cisco.com/search?query=ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa

Example with Process of Filename:

.. code::

    https://console.amp.cisco.com/search?query=tasksche.exe

Example with IP Address:

.. NOTE::

    Can be the IP address of a computer or the IP address a computer is connected to.

.. code::

    https://console.amp.cisco.com/search?query=82.165.37.127

Example with Domain:

.. code::

    https://console.amp.cisco.com/search?query=propay24.ru

Example with URL:

.. code::

    https://console.amp.cisco.com/search?query=http://propay24.ru/4/pict.jpg

Pivoting by Dashboard
---------------------

Example for the SHA-256 from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard?duration=720&artifact_type=sha&artifact=<SHA256>

Example for the IP Addresses from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard?duration=720&artifact_type=ip&artifact=<IP_ADDRESS>

Example for the Event Type from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard?duration=720&event_type=<EVENT_TYPE_ID>

Pivoting by Events
------------------

Example for the Event Type(s) from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard/overview#/events/show/{"filters":{"agg":[],"time":"all","tid":[<EVENT_TYPE_ID>]},"sort_by":"ts","sort_order":"desc","name":""}

Example for the Connector GUID from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard/overview#/events/show/{"filters":{"agg":[],"time":"all","tid":[],"ag":["<CONNECTOR_GUID>"]},"sort_by":"ts","sort_order":"desc","name":""}

Example for the Group GUID from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard/overview#/events/show/{"filters":{"time":["all"],"agg":["<GROUP_GUID>"]},"sort_by":"ts","sort_order":"desc","name":""}

Example for the SHA-256 from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard/overview#/events/show/{"filters":{"agg":[],"time":"all","tid":[],"sha":["<SHA256>"]},"sort_by":"ts","sort_order":"desc","name":""}

Example for the Detection Name from the last 30 days:

.. code::

    https://console.amp.cisco.com/dashboard/overview#/events/show/{"filters":{"agg":[],"time":"all","tid":[],"det_name":["<DETECTION>"]},"sort_by":"ts","sort_order":"desc","name":""}

Pivoting by File Trajectory
---------------------------

Example for pivoting by file trajectory:

.. code::

    https://console.amp.cisco.com/file/trajectory/<SHA256>

Pivoting by Device Trajectory
-----------------------------

Example to load to most recent event:

.. code::

    https://console.amp.cisco.com/computers/<CONNECTOR_GUID>/trajectory2

Example to load to specific event:

.. code::

    https://console.amp.cisco.com/computers/<CONNECTOR_GUID>/trajectory2?id=<EVENT_ID>

Example to filter to an observable:

.. code::

    https://console.amp.cisco.com/computers/<COMPUTER_GUID>/trajectory?q=<QUERY>

Example using SHA-256:

.. code::

    https://console.amp.cisco.com/computers/6c0c5f52-8992-4ae7-80c0-c10a3f3973b7/trajectory?q=ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa

Example using Process or Filename:

.. code::

    https://console.amp.cisco.com/computers/6c0c5f52-8992-4ae7-80c0-c10a3f3973b7/trajectory?q=tasksche.exe

Example using IP Address:

.. code::

    https://console.amp.cisco.com/computers/36b46210-30f6-4236-bbb2-5dbaa23947b6/trajectory?q=82.165.37.127

Example using Domain:

.. code::

    https://console.amp.cisco.com/computers/1d485168-407a-4b01-855c-20522f365046/trajectory?q=propay24.ru

Example using URL:

.. code::

    https://console.amp.cisco.com/computers/1d485168-407a-4b01-855c-20522f365046/trajectory?q=http://propay24.ru/4/pict.jpg






