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

To prompt a user for which command line switches they would like to use please see Chapter 3 of Deployment Strategy
Guide or Chapter 7 of the User Guide `here <https://console.amp.cisco.com/docs>`_ for a list of available command line
switches.

Upgrade Windows AMP for Endpoints Connector
"""""""""""""""""""""""""""""""""""""""""""

To upgrade the connector please read the command switches used during the previous installation from the following
local.xml files:

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

To get the AMP Install Dir go to: ``HKEY_LOCAL_MACHINE\SOFTWARE\Immunet Protect\InstallDir``

``$AMP_InstallDir\local.xml``

Default location is: ``C:\Program Files\Cisco\AMP\local.xml``

Read the value located here: ``/config/agent/uuid``

Linux
^^^^^

Go to here: ``/opt/cisco/amp/etc/local.xml``

Read the value located here: ``/config/agent/uuid``

MacOS
^^^^^

Go to here: ``/Library/Application Support/Cisco/AMP for Endpoints Connector/local.xml``

Read the value located here: ``/config/agent/uuid``

Uninstall
---------

Full Uninstall
^^^^^^^^^^^^^^

This action will uninstall AMP for Endpoints and remove all data from disk. If you later re-install AMP on the computer
it will register with a new GUID.

Windows
"""""""

To remove AMP from Windows please do the following:

1. Find the directory path for uninstall.exe ``%AMP_INSTALL_DIR\%VERSION``.
2. Navigate to the directory. Here is an example ``C:\Program Files\Cisco\AMP\7.2.7``.
3. Run the following command:

.. code::

    uninstall.exe /S /full 1 /password <PASSWORD>

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
disk. This will result in the connector re-registering with the cloud using the same GUID. This capabiliy is not
available for Mac OS.

Windows
"""""""

To remove AMP from Windows but leave the configuration please do the following:

1. Find the directory path for uninstall.exe ``%AMP_INSTALL_DIR\%VERSION``.
2. Navigate to the directory. Here is an example ``C:\Program Files\Cisco\AMP\7.2.7``.
3. Run the following command:

.. code::

    uninstall.exe /S /full 0 /password <PASSWORD>

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

Linux
"""""

Start agent in CentOS versions 6 and below:

.. code::

    initctl start cisco-amp

Start agent in CentOS versions 7 and above:

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

Stop agent with the following command by finding the directory path for sfc.exe ``%AMP_INSTALL_DIR\%VERSION``:

.. code::

    <FILE PATH> -k <PASSWORD>

Linux
"""""

Stop agent in CentOS versions 6 and below:

.. code::

    initctl stop cisco-amp

Stop agent in CentOS versions 7 and above:

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
help diagnose issue with an AMP deployment. You should only need to run this tool at the request of Cisco Support. This
task allows you to run the AMP Support Tool and upload the results to the BES Server through the BES Upload Manager. By
default the uploaded files will be placed in a subfolder under
``C:\Program Files\BigFix Enterprise\BES Server\UploadManagerData\BufferDir\sha1`` on a Windows BES Server or
``/var/opt/BESServer/UploadManagerData/BufferDir/sha1`` on a Linux BES Server. Each BES Client targeted will upload
approximately 1-250MB of data to the BES Server (through the BES Relays). Depending on network speeds, this could take
several minutes.

.. NOTE::

    The -o in the following commands is where the support snapshot will be saved.

Windows
"""""""

Run the following commands:

.. code::

    folder delete "<PathOfClientFolderOfCurrentSite>"
    folder create "<PathOfClientFolderOfCurrentSite>"
    "C:\Program Files\Cisco\AMP\7.2.7\ipsupporttool.exe" -o "<PathOfClientFolderOfCurrentSite>"
    setting "_BESClient_ArchiveManager_MaxArchiveSize"="262144000" on "<ActionIssueDate>" for client
    setting "_BESClient_ArchiveManager_OperatingMode"="2" on "<ActionIssueDate>" for client
    setting "_BESClient_ArchiveManager_FileSet-AMP"="<PathOfClientFolderOfCurrentSite>" on "<ActionIssueDate>" for client
    archive now

Linux
"""""

Run the following commands:

.. code::

    folder delete "<PathOfClientFolderOfCurrentSite>"
    folder create "<PathOfClientFolderOfCurrentSite>"
    "/opt/cisco/amp/bin/ampsupport" -o "<PathOfClientFolderOfCurrentSite>"
    setting "_BESClient_ArchiveManager_MaxArchiveSize"="262144000" on "<ActionIssueDate>" for client
    setting "_BESClient_ArchiveManager_OperatingMode"="2" on "<ActionIssueDate>" for client
    setting "_BESClient_ArchiveManager_FileSet-AMP"="<PathOfClientFolderOfCurrentSite>" on "<ActionIssueDate>" for client
    archive now

MacOS
"""""

Run the following commands:

.. code::

    folder delete "<PathOfClientFolderOfCurrentSite>"
    folder create "<PathOfClientFolderOfCurrentSite>"
    /Library/Application Support/Cisco/AMP for Endpoints Connector/SupportTool" -o "<PathOfClientFolderOfCurrentSite>"
    setting "_BESClient_ArchiveManager_MaxArchiveSize"="262144000" on "<ActionIssueDate>" for client
    setting "_BESClient_ArchiveManager_OperatingMode"="2" on "<ActionIssueDate>" for client
    setting "_BESClient_ArchiveManager_FileSet-AMP"="<PathOfClientFolderOfCurrentSite>" on "<ActionIssueDate>" for client
    archive now


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

To enable logging run the following command:

.. code::

    "<PathNameWithCiscoAMP>" -l start

Linux
"""""

To enable logging run the following commands:

.. code::

    delete __appendfile
    delete ampdebug.sh
    appendfile #!/bin/sh
    appendfile echo "debuglevel 1" | /opt/cisco/amp/bin/ampcli
    move __appendfile ampdebug.sh
    chmod 555 ampdebug.sh
    "<PathOfClientFolderOfCurrentSiteAnd ampdebug.sh>"

MacOS
"""""

To enable logging run the following commands:

.. code::

    delete __appendfile
	delete ampdebug.sh
	appendfile #!/bin/sh
	appendfile echo "debuglevel 1" | /opt/cisco/amp/ampcli
	move __appendfile ampdebug.sh
	chmod 555 ampdebug.sh
	"<PathOfClientFolderOfCurrentSiteAnd ampdebug.sh>"


Clear Cache
^^^^^^^^^^^

Windows
"""""""

To clear the cache run the following commands and find the directory path for sfc.exe ``%AMP_INSTALL_DIR\%VERSION``:

.. code::

    <FILE PATH> -k <PASSWORD>
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

To clear cache in CentOS versions 6 and below use the following commands:

.. code::

    initctl stop cisco-amp
    rm -f "/opt/cisco/amp/etc/cloud_query.cache"
    rm -f "/opt/cisco/amp/etc/cloud_nfm_query.cache"
    rm -f "/opt/cisco/amp/etc/events.db"
    initctl start cisco-amp

To clear cache in CentOS versions 7 and above use the following commands:

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


