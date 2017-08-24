## Overview
The final two elements of preparing our Windows Server virtual machine is to install Internet Information Server (IIS) and configure the necessary port (80) in the fireall to allow HTTP requests.

## Install IIS
Since, for this example, we will be deploying and hosting a basic, static website, the standard IIS components are sufficient.  We could install them through the Server Manager, but we are going to use PowerShell so that we become familiar with executing tasks for later when we need to automate this process in Docker.

!!<h4>IIS Management Console</h4>In this step, we will install the IIS Management Console.  However, when we build our container, we will use Microsoft Nano Server as the base image. Nano Server has no UI.  Therefore, keep in mind that when we build our Dockerfile, we will <strong>not</strong> include the management interface.

  1. Open PowerShell in elevated mode (with Administrator privileges):
     <img src="../images/pwershell-admin.png" class="block"/>
  2. Type the command `Install-WindowsFeature -Name Web-Server, Web-Mgmt-Tools`
  3. You should then see the components download and install.
     <img src="../images/install-iis.jpg" class="block"/>

## Configure Firewall
The last step to configuring our server is to allow IIS to serve webpages through port 80.  By default, the port is blocked and so, even if IIS was running, we would not be able to access the site outside of the server, itself.  We, again, are going to use PowerShell to configure the firewall.

  1. If it's not already open, again open PowerShell in elevated mode.
  2. Type the following command `New-NetFirewallRule -DisplayName 'HTTP(S) Inbound' -Profile @('Domain', 'Private', 'Public') -Direction Inbound -Action Allow -Protocol TCP -LocalPort @('80', '443')`
     <img src="../images/firewall-rule.jpg" class="block"/>

We've now completed the server setup. We could configure a separate IIS site and app pool for our site.  But, to keep things simple, we're going to use the default.