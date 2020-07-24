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

