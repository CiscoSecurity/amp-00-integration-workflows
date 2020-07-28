Managed Deployment Techniques
=============================

Deploy Agent
------------

Cisco AMP supports managed installs and has different command line flags/switches that can be used to customize agent
installation, which vary by operating system.

Windows
^^^^^^^

.. NOTE::

    Common flags and what they do:

    - /R: For all Connector versions 5.1.13 and higher this must be the first switch used.
    - /S: Used to put the installer into silent mode.
    - /skiptetra 1: Skip installation of the TETRA driver.
    - /skipdfc 1: Skip installation of the DFC driver.

    ``/skiptetra`` and ``/skipdfc`` are both binary switches where 0 is false/off and 1 is true/on.

    For more information please see Chapter 3 of Deployment Strategy Guide or Chapter 7 of the User Guide `here <https://console.amp.cisco.com/docs>`_.

Deploy Windows AMP for Endpoint With Default Switches
"""""""""""""""""""""""""""""""""""""""""""""""""""""

When installing on a Windows Server/Domain Controller:

.. code::

    amp_GroupName.exe /R /S /skiptetra 1 /skipdfc 1

When installing on Windows Desktops:

.. code::

    amp_GroupName.exe /R /S

Deploy Windows AMP for Endpoint With No UI Elements
"""""""""""""""""""""""""""""""""""""""""""""""""""

When installing on a Windows Server/Domain Controller:

.. code::

    amp_GroupName.exe /R /S /skiptetra 1 /skipdfc 1 /desktopicon 0 /startmenu 0 /contextmenu 0

When installing on Windows Desktops:

.. code::

    amp_GroupName.exe /R /S /desktopicon 0 /startmenu 0 /contextmenu 0

Deploy Windows AMP for Endpoint and Specify the Installation Parameters
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

For a complete list of command line switches that can be used during installation please see Chapter 3 of Deployment
Strategy Guide or Chapter 7 of the User Guide `here <https://console.amp.cisco.com/docs>`_. You can then prompt the user
for the value of each switch.

Upgrade Windows AMP for Endpoints Connector
"""""""""""""""""""""""""""""""""""""""""""

To upgrade the connector please read the command line switches used during the previous installation from the ``local.xml``:

.. NOTE::

    The ``local.xml`` is found in Cisco AMP install directory which can be found by checking the registry key value of ``HKEY_LOCAL_MACHINE\SOFTWARE\Immunet Protect\InstallDir``.

- ``/config/install/switches/skipdfc``
- ``/config/install/switches/skiptetra``
- ``/config/install/switches/skipexprevprereqcheck``
- ``/config/install/switches/desktopicon``
- ``/config/install/switches/startmenu``
- ``/config/install/switches/contextmenu``
- ``/config/install/switches/overridepolicy``

Read the command switches used during the previous installation from the local.xml. For example if the local.xml contained:

.. code::

    ...
      </agent>
      <install>
       <switches>
        <skipdfc>1</skipdfc>
        <desktopicon>0</desktopicon>
        <sendfile>1</sendfile>
        <versioncheck>0</versioncheck>
        <noadmin>0</noadmin>
        <skiposcheck>0</skiposcheck>
        <skiptetra>1</skiptetra>
        <contextmenu>0</contextmenu>
        <startmenu>0</startmenu>
        <trayicon>0</trayicon>
        <overridepolicy>1</overridepolicy>
        <skipexprevprereqcheck>0</skipexprevprereqcheck>
        <overrideinstpathlength>0</overrideinstpathlength>
        <renameinstalldir>1</renameinstalldir>
       </switches>
      </install>
      <janus>
    ...

Then you would use the following command to upgrade:

.. code::

    amp_GroupName.exe /R /S /skipdfc 1 /skiptetra 1 /skipexprevprereqcheck 0 /desktopicon 0
    /startmenu 0 /contextmenu 0 /overridepolicy 1


Linux
^^^^^

Deploy Linux AMP for Endpoint Connector
"""""""""""""""""""""""""""""""""""""""

For RHEL/CentOS versions 6-8 please go `here <https://console.amp.cisco.com/download_connector>`_ and select the group
you will be deploying a connector for. Next, select the distribution of Linux you will be using and copy the URL it
creates. Then, run the following two commands:

.. code::

    wget <CopiedURL> -o amp_<GroupName>_rhel-<LinuxDistribution>.rpm
    yum install -y amp_<GroupName>_rhel-<LinuxDistribution>.rpm

Upgrade Linux AMP for Endpoints Connector
"""""""""""""""""""""""""""""""""""""""""

To upgrade RHEL/CentOS versions 6-8 connectors please go `here <https://console.amp.cisco.com/download_connector>`_ and select the group
for the connector. Next, select the distribution of Linux that was used and copy the URL it
creates. Then, run the following two commands:

.. code::

    wget <CopiedURL> -o amp_<GroupName>_rhel-<LinuxDistribution>.rpm
    yum install -y amp_<GroupName>_rhel-<LinuxDistribution>.rpm


MacOS
^^^^^

Deploy MacOS AMP for Endpoint Connector
"""""""""""""""""""""""""""""""""""""""

Once you have the connector on the endpoint, execute the following commands to install:

Please modify the file name to whatever the file was saved as.

.. code::

    hdiutil attach amp_GroupName.dmg
    installer -pkg /Volumes/ampmac_connector/ciscoampmac_connector.pkg -target /
    hdiutil detach /Volumes/ampmac_connector

Upgrade MacOS AMP for Endpoints Connector
"""""""""""""""""""""""""""""""""""""""""

To upgrade, get the connector on the endpoint and execute the following commands to install:

Please modify the file name to whatever the file was saved as.

.. code::

    hdiutil attach amp_GroupName.dmg
    installer -pkg /Volumes/ampmac_connector/ciscoampmac_connector.pkg -target /
    hdiutil detach /Volumes/ampmac_connector



Check Agent Status
------------------

Windows
^^^^^^^

Installation Status
"""""""""""""""""""

To confirm installation was successful look for a service that contains the string ``CiscoAMP``.

Connector Status
""""""""""""""""

To find the connector version and connector state for Windows computers with the AMP for Endpoints connector you first
check the version of a service that contains ``CiscoAMP``. Then, you check if a running service contains ``CiscoAMP``.

Linux
^^^^^

Installation Status
"""""""""""""""""""

To check if the AMP connector is installed check for the following file ``/opt/cisco/amp/bin/ampdaemon``.

Connector Status
""""""""""""""""

To find the connector version and connector state for Linux computers with the AMP for Endpoints connector you first
check ``/opt/cisco/amp/etc/global.xml`` for the version. Then, see if the service is running under a process named
``ampdaemon``.

MacOS
^^^^^

Installation Status
"""""""""""""""""""

To check if the AMP connector is installed check for the following file ``/opt/cisco/amp/ampdaemon``.

Connector Status
""""""""""""""""

To find the connector version and connector state for MacOS computers with the AMP for Endpoints connector you first
check ``/opt/cisco/amp/global.xml`` for the version. Then, see if the service is running under a process named
``ampdaemon``.

Get Agent GUID
--------------

Windows
^^^^^^^

To get the AMP InstallDir check the registry key value of ``HKEY_LOCAL_MACHINE\SOFTWARE\Immunet Protect\InstallDir``.

Read the value of ``/config/agent/uuid`` from ``$AMP_InstallDir\local.xml``.

Default location is: ``C:\Program Files\Cisco\AMP\local.xml``

Linux
^^^^^

Read the value of ``/config/agent/uuid`` from ``/opt/cisco/amp/etc/local.xml``.

MacOS
^^^^^

Read the value of ``/config/agent/uuid`` from ``/Library/Application Support/Cisco/AMP for Endpoints Connector/local.xml``.

Uninstall
---------

Full Uninstall
^^^^^^^^^^^^^^

This action will uninstall AMP for Endpoints and remove all data from disk. If you later re-install AMP on the computer
it will register with a new GUID.

Windows
"""""""

To remove AMP from Windows please do the following:

1. Find the directory path for the ``uninstall.exe`` ``%AMP_InstallDir\%VERSION`` by checking the image path of the Cisco AMP for Endpoints process. The Service name will be ``CiscoAMP_%VERSION``. The image path will be ``%AMP_InstallDir\%VERSION\sfc.exe``.
2. Navigate to the directory. Here is an example ``C:\Program Files\Cisco\AMP\7.2.7``.
3. Run the following command:

.. code::

    uninstall.exe /S /full 1 /password <PASSWORD>

.. NOTE::

    The ``/password`` switch is only required if a Connector Protection Password is configured. If it is not provided the
    ``/password`` switch is ignored.

Linux
"""""

To remove AMP from Linux please run these commands:

.. code::

    yum remove ciscoampconnector -y
    /opt/cisco/amp/bin/purge_amp_local_data


MacOS
"""""

To remove AMP from MacOS please run this command:

.. code::

    installer -pkg "/Applications/Cisco AMP/Uninstall AMP for Endpoints Connector.pkg" -target /



Uninstall But Leave Configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you plan to re-install AMP for Endpoints at a later date you should use this action to leave configuration on the
disk. This will result in the connector re-registering with the cloud using the same GUID. This capability is not
available for Mac OS.

Windows
"""""""

To remove AMP from Windows but leave the configuration please do the following:

1. Find the directory path for ``uninstall.exe`` ``%AMP_InstallDir%VERSION`` by checking the image path of the Cisco AMP for Endpoints process. The Service name will be ``CiscoAMP_%VERSION``. The image path will be ``%AMP_InstallDir%VERSION\sfc.exe``.
2. Navigate to the directory. Here is an example ``C:\Program Files\Cisco\AMP\7.2.7``.
3. Run the following command:

.. code::

    uninstall.exe /S /full 0 /password <PASSWORD>

.. NOTE::

    The ``/password`` switch is only required if a Connector Protection Password is configured. If it is not provided the
    ``/password`` switch is ignored.

Linux
"""""

To remove AMP from Linux but leave the configuration please run this command:

.. code::

    yum remove ciscoampconnector -y



Starting and Stopping Agents
----------------------------

Starting Agents
^^^^^^^^^^^^^^^

Windows
"""""""

Start agent with the following command:

.. code::

    cmd.exe /c "net start Cisco AMP for Endpoints Connector 7.2.7"

.. NOTE::

    To get the name of the service check for a Service name that starts with ``CiscoAMP_``.

Linux
"""""

Start agent in RHEL/CentOS versions 6 and below:

.. code::

    initctl start cisco-amp

Start agent in RHEL/CentOS versions 7 and above:

.. code::

    systemctl start cisco-amp

MacOS
"""""

Start agent with the following command:

.. code::

    launchctl load /Library/LaunchDaemons/com.cisco.amp.daemon.plist


Stopping Agents
^^^^^^^^^^^^^^^

Windows
"""""""

Find the directory path for sfc.exe ``%AMP_InstallDir%VERSION`` by checking the image path of the Cisco AMP for
Endpoints process. The Service name will be ``CiscoAMP_%VERSION``. The image path will be
``%AMP_InstallDir%VERSION\sfc.exe``. To stop the agent run the following command:


.. code::

    <FILE PATH> -k <PASSWORD>

.. NOTE::

    The ``-k`` switch is only required if a Connector Protection Password is configured. If it is not provided the ``-k`` switch is ignored.

Linux
"""""

Stop agent in RHEL/CentOS versions 6 and below:

.. code::

    initctl stop cisco-amp

Stop agent in RHEL/CentOS versions 7 and above:

.. code::

    systemctl stop cisco-amp

MacOS
"""""

Stop agent with the following command:

.. code::

    launchctl unload /Library/LaunchDaemons/com.cisco.amp.daemon.plist

Troubleshooting
---------------

Support Tools
^^^^^^^^^^^^^

The AMP Support Tool will create a snapshot of system and AMP settings include AMP logs to be used by Cisco support to
help diagnose issue with an AMP deployment. You should only need to run this tool at the request of Cisco Support.

.. NOTE::

    The ``-o`` in the following commands is where the support snapshot will be saved.

Windows
"""""""

Find the directory path for ``ipsupporttool.exe`` ``%AMP_InstallDir%VERSION`` by checking the image path of the Cisco AMP for
Endpoints process. The Service name will be ``CiscoAMP_%VERSION``. The image path will be ``%AMP_InstallDir%VERSION\sfc.exe``.
Then run the following command:

.. code::

    "C:\Program Files\Cisco\AMP\7.2.7\ipsupporttool.exe" -o "<DesiredOutputDirectory>"

Linux
"""""

Run the following command:

.. code::

    "/opt/cisco/amp/bin/ampsupport" -o "<DesiredOutputDirectory>"

MacOS
"""""

Run the following command:

.. code::

    /Library/Application Support/Cisco/AMP for Endpoints Connector/SupportTool" -o "<DesiredOutputDirectory>"

Reboot Required
^^^^^^^^^^^^^^^

To check if AMP needs a Windows Client to Reboot, look for the following registry key:
``HKEY_LOCAL_MACHINE\SOFTWARE\Immunet Protect\Reboot``. Reboot Windows machines that have a pending reboot caused by
AMP for Endpoints. Pending reboots can be caused by an upgrade or an uninstallation.

Enable Debug Logging
^^^^^^^^^^^^^^^^^^^^

.. NOTE::

    Debug logging will automatically turn off after the next policy update.

Windows
"""""""

Find the directory path for ``sfc.exe`` ``%AMP_InstallDir%VERSION`` by checking the image path of the Cisco AMP for
Endpoints process. The Service name will be ``CiscoAMP_%VERSION``. The image path will be
``%AMP_InstallDir%VERSION\sfc.exe``. To enable logging run the following command:

.. code::

    sfc.exe -l start

Linux
"""""

To enable logging run the following command:

.. code::

    echo "debuglevel 1" | /opt/cisco/amp/bin/ampcli

MacOS
"""""

To enable logging run the following commands:

.. code::

	echo "debuglevel 1" | /opt/cisco/amp/ampcli


Clear Cache
^^^^^^^^^^^

Windows
"""""""

Find the directory path for sfc.exe ``%AMP_InstallDir%VERSION`` by checking the image path of the Cisco AMP for
Endpoints process. The Service name will be ``CiscoAMP_%VERSION``. The image path will be
``%AMP_InstallDir%VERSION\sfc.exe``. To clear the cache run the following commands:

.. NOTE::

    You can get the Cisco AMP install directory by checking the registry key value of ``HKEY_LOCAL_MACHINE\SOFTWARE\Immunet Protect\InstallDir``.

.. NOTE::

    The ``-k`` switch is only required if a Connector Protection Password is configured. If no is provided the ``-k`` switch is ignored.

.. NOTE::

    To get the name of the service check for a Service name that starts with ``CiscoAMP_``.

.. code::

    sfc.exe -k <PASSWORD>
    delete "C:\Program Files\Cisco\AMP\cache.db"
    delete "C:\Program Files\Cisco\AMP\nfm_cache.db"
    delete "C:\Program Files\Cisco\AMP\nfm_url_file_map.db"
    delete "C:\Program Files\Cisco\AMP\event.db"
    delete "C:\Program Files\Cisco\AMP\jobs.db"
    delete "C:\Program Files\Cisco\AMP\history.db"
    delete "C:\Program Files\Cisco\AMP\historyex.db"
    powershell.exe Start-Service <ServiceNameOfCiscoAMP>


Linux
"""""

To clear cache in RHEL/CentOS versions 6 and below use the following commands:

.. code::

    initctl stop cisco-amp
    rm -f "/opt/cisco/amp/etc/cloud_query.cache"
    rm -f "/opt/cisco/amp/etc/cloud_nfm_query.cache"
    rm -f "/opt/cisco/amp/etc/events.db"
    initctl start cisco-amp

To clear cache in RHEL/CentOS versions 7 and above use the following commands:

.. code::

    systemctl stop cisco-amp
    rm -f "/opt/cisco/amp/etc/cloud_query.cache"
    rm -f "/opt/cisco/amp/etc/cloud_nfm_query.cache"
    rm -f "/opt/cisco/amp/etc/events.db"
    systemctl start cisco-amp

MacOS
"""""

To clear cache in MacOS run the following commands:

.. code::

    launchctl unload /Library/LaunchDaemons/com.cisco.amp.daemon.plist
    rm -f "/Library/Application Support/Cisco/AMP for Endpoints Connector/cloud_query.cache"
    rm -f "/Library/Application Support/Cisco/AMP for Endpoints Connector/cloud_nfm_query.cache"
    rm -f "/Library/Application Support/Cisco/AMP for Endpoints Connector/events.db"
    launchctl load /Library/LaunchDaemons/com.cisco.amp.daemon.plist


