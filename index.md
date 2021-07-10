==InstaSafe Zero Trust Application Access Documentation ==



=== Introduction ===

Popular legacy based security models these days, including the VPN, run on the principle of trusting “inside the network” users and assets by default. With the fast adaptation of hybrid users & remote working norms, the authentication of users can be easily compromised, making conventional security models highly vulnerable. 

Therefore, There is a need for a system that reimagines network security as we know it.Hence, the idea of Zero Trust came into existence.

 The documentation covers the Zero Trust solution and a detailed explanation of the administrator and the user perspective.

What is Zero Trust?

Zero Trust is a security model that doesn't trust any entity, whether inside or outside the network. Unlike traditional security systems which follow the 'Trust but verify' approach, Zero Trust Models follow a 'Never Trust, always verify' approach, using strict access control policies and constant monitoring to secure enterprise networks from malware and other security threats. Zero trust ensures that each and every user and their devices are validated and given the least required access on a “need to know” basis. It also involves continuous monitoring of current users to identify malicious behaviour and revoke access accordingly.

Traditional network centric security systems grant more trust than required, which can be exploited.


Zero Trust Models

Keeping in mind about the above principles of Zero Trust, Companies have implemented Zero Trust into their products in two different ways

Client Initiated Zero Trust Access
This model uses the Software Defined Perimeter as proposed by the Cloud Security Alliance as its inspiration. This model includes the use of an “Agent” which resides on the client’s machines and connects the client to  the required resource after performing necessary security checks and validations defined by the company operating the Zero Trust Network.
Such a model can be used to access company resources in private networks  and cloud hosted services securely from anywhere in the world. 
Examples of client-initiated solutions are InstaSafe’s Zero Trust Application Access (ZTAA) and Zero Trust Network Access (ZTNA).

<pic>

Service Initiated Zero Trust Access
This model implements Zero Trust using a reverse proxy architecture. It is based on Google’s Zero Trust Implementation called BeyondCorp. It uses the browser to validate the user and  establish a tunnel between the client and server eliminating the need for an agent to be installed and maintained on enterprise devices. However, The access is limited to cloud hosted services as it only supports protocols like HTTP/HTTPs due to its reliance on the browser and has limited support for other protocols.
Examples of client-initiated solutions are Google’s BeyondCorp InstaSafe’s Agentless Gateway
<pic>

InstaSafe’s Zero Trust Products

InstaSafe offers two different Zero Trust based remote access solutions


InstaSafe Zero Trust Application Access (ZTAA)

Instasafe’s ZTAA is based on the Client-Gateway model in Software Defined Perimeter specification as proposed by the  Cloud Security Alliance (CSA) . The design follows the rules put forward by NIST as well. It is a client-initiated ZTNA solution which creates a secure tunnel between the client and the server with additional  features like Multi-Factor Authentication and integration with SAML for third party applications. 


The architecture follows the SPA and drop-all-firewall principles in order to hide the IT infrastructure completely from the internet. Any network request from an unknown client will be dropped off (not deny) by the firewall rules. Hence the presence of the ZT infrastructure and assets are hidden from the internet.

Link: <website>



InstaSafe Zero Trust Network Access (ZTNA)

InstaSafe’s ZTNA is also a client-initiated ZTNA solution which instead of connecting a client to an application connects the client to a network via a fast and secure tunnel. It also includes features like Multi-Factor Authentication and device profiling and validation similar to ZTAA.
Link: <website>




Architecture

Zero Trust Application Access -  Application in DMZ

Overview 

The architecture is based on the Client-Gateway model as detailed in the CSA architecture guide for SDP .

Components

The primary components of the ZTAA include the Client,  Controller,  Gateway and the IT resources of the organisation. 

Clients are devices that are in the hands of users who wish to access resources which are secured by  ZTAA. Laptops, Desktops, Mobile phones are examples of such clients. However, the ZTAA Agent has to be installed on these devices to be able to communicate and interact with the Zero Trust Network . The client is configured to drop all connections to the SDP in the case that any of  ZTAA’s access policies are violated or the standards are not met. 

Controllers are the decision making components in ZTAA . They are connected to Identity providers  to gather information about the users trying to access the network. The client sends vital information about the user’s device to the controller which helps the controller in granting entitlements i.e the level of access to the clients. In the context of the CSA SDP model, this component is the Policy Decision Point (PDP).

Gateways are the components which enforce the policies and entitlements set by the controllers. They verify the client's entitlements to grant them access to the resources only in the client’s context. In the context of the CSA SDP or NIZT ZT model, this component is the Policy Enforcement Point (PEP)

Resources are the infrastructure that are secured by ZTAA.





                                                Figure 1

The client connects to the desired resource as graphically depicted in Figure 1.

The client sends a Single Packet Authentication (SPA) packet containing its device and user fingerprint to the controller. The controller will validate that SPA packet. On successful validation, a dynamic port rule is opened such that the client’s ip can connect to the controller but no response is sent to the client.
The client then connects to the controller via a secure control channel separate from the data channel used to transmit application data. 
The client provides the controller with data about the device’s various security measures as defined in the controller’s trust policy. The controller uses an identity provider to authenticate the user’s credentials. Additionally, Multi-Factor-Authentication is also used to verify the user’s authenticity.
Based on the information gathered on the user and user’s device, the controller issues a dynamic entitlement token which is cryptographically signed by itself to the client. The entitlement is dynamic because the user is not guaranteed the same entitlements on each access if the client or the device does not meet certain policy rules. The entitlement is granted based on the data collected from the sources at the time of access, therefore is subject to change.
 After receiving the entitlement in the form of a certificate, the client then connects to the gateway by using another SPA mechanism and provides its certificate to access a particular resource. The gateway verifies whether the certificate was issued by the controller. The gateway then uses the entitlement to determine whether the requested resources are within the client’s context by communicating with the controller.
On successful validation of the client’s access request, a mTLS tunnel is established to the gateway and the data flows from the application to the gateway on the local network and then is relayed to the client by the gateway through the TLS tunnel. The gateway dynamically creates access rules for the client to use the resources.This is the first part of the process where the user is allowed into ZTAA’s network to access critical resources. Therefore, the gateway logs and monitors all the traffic flowing in and out of the network. 


	


Zero Trust Application Access -  Third Party Applications (SAML based)
Overview 

The architecture is based on the interaction between a SAML Service Provider and SAML Identity Provider with the additional layers of security that come with using the Zero Trust Platform.

Components
The primary components of the SAML based ZTNA access include a Client, a Controller, a Gateway and the resources. 

Clients are devices that are in the hands of users who wish to access resources which are secured by  ZTAA. Laptops, Desktops, Mobile phones are examples of such clients. An agent or the web browser can be used to access applications of this type

Controllers are the decision making components in ZTAA . They are connected to Identity providers to gather information about the users trying to access the network. The client sends vital information about the user’s device to the controller which helps the controller in granting entitlements i.e the level of access to the clients. The controller also houses an Identity Provider which can be used for logging into SaaS applications that support Single Sign On.

SaaS Application  is the third-party hosted application secured by the ZTAA architecture. It logs the user into the application after receiving an Identity assertion from the Identity Provider

SAML Service Provider (SP)  is a part of the SaaS application delivery  which sends an SAML AuthNRequest to the Identity Provider to authenticate a user.

It receives and validates the SAML Response sent by the Identity Provider. On successful validation, it redirects the user to the authenticated SaaS application.

SAML Identity Provider (IdP) is a component of Zero Trust Platform which provides authentication identities of users within the system to third party applications to enable Single Sign On into those applications.


Authentication Flows

SAML Single Sign On (SAML SSO) has two authentication flows which allow developers to use the flow more suitable to their business requirements.
IdP Initiated ( Identity Provider Initiated)
This flow is initiated by the identity provider. Once the SP is preconfigured with all the data necessary to establish trust between the SP and IdP, the IdP sends a SAML Response to the SP with the authentication assertions of a users and the SP validates the response and forwards and authenticated application to the user on successful validation of the response.

In ZTAA, this flow is useful when providing the one-click interface to the user to access the third party applications. The flow inside ZTAA is as follows
User logins into the  ZTAA platform using the agent / web browser (user agent) and navigates to the Application Access screen. The user clicks on any of the third party applications.
Since the user is already authenticated by the ZTAA platform, the IdP sends a SAML Response to the Service Provider providing the assertions about the user to validate the user’s identity.
The Service Provider receives and validates the SAML Response sent by the IdP via the user agent.
On successful validation of the SAML Response, the Service Provider returns an authenticated session of the application to the user agent for the user to consume.

The visualization of the above flow can be seen in the following figure

					

SP Initiated ( Service Provider Initiated)
This flow is initiated by the Service Provider.  The SP is preconfigured with all the data necessary to establish trust between the SP and IdP. The Service Provider sends a AuthNRequest to the IdP requesting for identity assertions. The IdP sends a SAML Response to the SP with the authentication assertions of the users and the SP validates the response and forwards an authenticated application session to the user on successful validation of the response.

In ZTAA, this flow is useful when users try to log into a third party application by clicking on the login with the SSO option provided by the application. The flow is as follows
The user is logged into the ZTAA platform. The user tries to login into a third party application using the SSO feature.
The application sends an AuthNRequest to the IdP requesting for assertions about the user’s identity.
Since the user is already authenticated by the ZTAA platform, the IdP sends a SAML Response to the Service Provider providing the assertions about the user to validate the user’s identity.
The Service Provider receives and validates the SAML Response sent by the IdP via the user agent.
On successful validation of the SAML Response, the Service Provider returns an authenticated session of the application to the user agent for the user to consume.

The visualization of the above flow can be seen in the following figure



 


Zero Trust Network Access 
Overview 

The architecture follows the Zero Trust ideology like ZTAA but unlike ZTAA, connects users to corporate networks or IP. The access can be leveraged for a set of IPs or can be bound to an IP and port.


Components

The primary components of the ZTNA include a Client, a Controller, a Gateway and resources. 

Clients are devices that are in the hands of users who wish to access resources which are secured by  ZTNA. Laptops, Desktops, Mobile phones are examples of such clients. However, the ZTAA Agent has to be installed on these devices to be able to communicate and interact with the Zero Trust Network . The client is configured to drop all connections to the SDP in the case that any of  ZTNA’s access policies are violated or the standards are not met. 

Controllers are the decision making components in ZTAA . They are connected to Identity providers like Radius to gather information about the users trying to access the network. The client sends vital information about the user’s device to the controller which helps the controller in granting entitlements i.e the level of access to the clients. In the context of the CSA SDP model, this component is the Policy Decision Point (PDP).

Gateways are the components which enforce the policies and entitlements set by the controllers. They verify the client's entitlements to grant them access to the resources only in the client’s context. In the context of the CSA SDP model, this component is the Policy Enforcement Point (PEP)

Resources are customer infrastructure elements that are secured by ZTNA.



                                                Figure 2

The client connects to the corporate network as graphically depicted in Figure 2.

The client sends a Single Packet Authentication (SPA) packet containing its device and user fingerprint to the controller. The controller will validate that SPA packet. On successful validation, a dynamic port rule is opened such that the client’s ip can connect to the controller but no response is sent to the client.
The client then connects to the controller via a secure control channel separate from the data channel used to transmit application data. 
The client provides the controller with data about the device’s various security measures as defined in the controller’s trust policy. The controller uses an identity provider to authenticate the user’s credentials. Additionally, Multi-Factor-Authentication is also used to verify the user’s authenticity.
Based on the information gathered on the user and user’s device, the controller issues a dynamic entitlement token which is cryptographically signed by itself to the client. The entitlement is dynamic because the user is not guaranteed the same entitlements on each access if the client or the device does not meet certain policy rules. The entitlement is granted based on the data collected from the sources at the time of access, therefore is subject to change.
 After receiving the entitlement in the form of a certificate, the client then connects to the gateway by using another SPA mechanism and provides its certificate to access a particular resource. The gateway verifies whether the certificate was issued by the controller. The gateway then uses the entitlement to determine whether the requested resources are within the client’s context by communicating with the controller.
On successful validation of the client’s access request, a wireguard tunnel is established from the client to the gateway and the data flows from the applications to the gateway on the local network and then is relayed to the client by the gateway through the Wireguard tunnel. The gateway dynamically creates access rules for the client to use the resources. The wg0 interface created by the wireguard tunnel can be extensively configured by the means of iptables access rules to achieve microsegmentation of the network  and allow access to machines on the network on a need-to-know basis . Therefore, the gateway logs and monitors all the traffic flowing in and out of the network.




Components


Clients

The Instasafe Agent is the primary way a consumer interacts with a Zero Trust Network . The Agent is available on all desktop operating systems as  an electron application. The agent is designed to access both the ZTAA infrastructure and ZTNA infrastructure at the same time. The agent provides a simple one click interface to the consumer to open applications configured in their company’s Zero Trust infrastructure. 


In the context of ZTAA, When the user clicks the application, the agent automates the entire process of communication between gateways and controllers and opens the respective application within the agent. To accommodate several application protocols, The agent has an inbuilt web browser, an open-source RDP client and a SSH client to access applications through the agent’s interface.

In the context of ZTNA, When the user starts the application, the agent establishes a wireguard tunnel between the consumer’s machine and the gateway configured. Once connected, the consumer will be able to access the applications on the configured network according to their privileges. Consumers will be able to access enterprise applications of their corporate network through the applications installed on their base machine.

The agent is available for download after logging into the Web application *.app.instasafe.io, and navigating to the downloads section.
The agent is configured to auto-update to the latest version available.


In addition to the desktop agent, Instasafe also offers mobile applications to access to the consumer to access their Zero Trust Infrastructure.
InstaSafe® 4T Ready Client is available on both the Google Play Store and the iOS App Store.

Policies
Policies are the way to control a user’s access to company infrastructure. They are a critical component in any zero trust network and have to be configured carefully keeping in mind the need-to-know model of allowing access.

Users
There are two types of users within the system.
User 
Every person using the platform will have the privilege of a user.
 A user can only use the platform and cannot make any changes to the platform
Admin
Admin is a privileged user who has the permissions to
Create and manage users and user groups
Create applications
Add Gateways
Add access policies
Etc..
Note: Navigate to the Admin UI section of this document to know the extent of privileges of an admin user.



Installation Guide



Zero Trust Application Access -  Applications in the DMZ

This guide details how to quickly set up a working ZTAA environment.
The following steps have to be performed by an admin user on the portal <companyname>.app.instasafe.io.

Create Users
To create users, Navigate to the Users tab under Identity Management.
 Click on the ‘+’ icon to add a new user. Details like the role of the user, contact details have to be provided on this screen. On successful addition of the user, you will be given a notification in green. 
You can verify the addition by refreshing the users page.

Create Applications
To add applications, navigate to the Applications tab under Perimeter Management.
Click on the ‘+’ icon to add a new application.Click on the application type required. Give the application an appropriate name and give its connection details like IP address (local) and Port.On successful addition of the user, you will be given a notification in green. 

Create Policies
To add policies, Navigate to Access Policies in menu bar on the left side
Click on the ‘+’ icon to add a new access policy. Give the policy an appropriate name and description and click on Next.
Click on the Add User button to add a user/users to the policy.
After adding user/users, Click on Next. 
This panel lets you add applications to the policy. This essentially means that all the users added to this policy will be able to access the applications that are added to this policy. Click on Add Application and add applications to the policy. And Click on next.
This panel allows the admin to configure specific rules and rulesets (a boolean combination of rules) that have to be met before being able to access the applications covered under this policy. Click on Next.
This panel allows you to configure OTP/Captcha access to the application in addition to the MFA during the initial login. Click on Next
This panel has the submit button to add this policy to the controller.On successful addition of the policy, you will be given a notification in green. 

Create a Gateway
The gateway is the relay point between the user and the application and point of access to all the applications secured by the ZTAA. Therefore, the security configuration of the gateway has to be impenetrable.

As an admin, Navigate to the Downloads page in the portal. Here you find the command to install the gateway on a linux machine
Prerequisites of the linux machine
A Linux machine which is publicly accessible has to be earmarked for the installation of the gateway software. 
This machine should be able to access all the applications that are to be accessed through the ZTAA network locally.
The machine should allow all inbound and outbound TCP traffic on port 443 publicly. It is strongly recommended that no ports other than 22 and 443 are opened publicly.
A user in the linux system with sudo privileges.
Once the prerequisites are met, SSH into the linux machine using the SSH client and authentication mechanism of your choice.
The installation script uses the curl command to download the installation script. Therefore, make sure that curl is installed on the system by runnin the following command
For Ubuntu and Debian systems
sudo apt install curl

For  RHEL, CentOS and Fedora distros
yum install curl


Once installed, we can run the gateway installation script. Before running the script, prepend the command in downloads with the keyword ‘sudo’. 
sudo /bin/bash -c "$(curl -fSsL https://storage.googleapis.com/4tr/start.sh) --devmode" 2>&1 | tee -a installationlogs.txt 

After some processing, you will be presented with the following menu

Enter 3 and press Enter. The following menu will appear

Enter 1 and press Enter. The installation process will begin.The next prompt will ask you to select the version of the gateway.
	
 Select the desired version by entering its corresponding number on the prompt and press Enter.
The installation process will start. It will download and install all the required packages. Once the installation is done, you will be presented with an access code as shown below.

Copy the access code here and head to the portal.
Navigate to the Gateways tab under Perimeter Management. Click on the ‘+’ to add a gateway. Select the option Private Gateway and click on Next.
On this panel, paste the access code that was generated by the gateway installation script and click on Next.
Finalize the installation by clicking on Authorize.
On successful addition of the gateway, you will be given a notification in green. 
The access code expires after a minute after it has been generated. Therefore, it is essential that the last few steps be performed quickly.
If the access code expires, follow all the steps until again step 8 where you are presented with an Install,Unistall screen. Uninstall the gateway and follow the gateway installation guide again to generate a new access code.


	
Add Applications to the Gateway
Now that the gateway has been added, the last part of the portal configuration is to add the applications to the gateway.
Navigate to the Gateways tab under Perimeter Management. Click on the gateway that has just been installed. The names of the gateways are the same as the hostname of the linux box they are hosted on.
On clicking the gateway, it expands to show more information and options. Here navigate to the Applications tab. Find View/Edit toggle on the topmost part of the screen and click to toggle between read and edit mode. 
Once in edit mode, Click on Add Applications, add the applications that are to be added to the gateway. Remember that the gateway can only access applications that are on the same VNC or network.
On successful addition of the application to the  gateway, you will be given a notification in green.

The ZTAA setup is complete. Users who were added in policies will be able access the applications through the agent. 
A guide for users to access applications can be found under the Getting Started section of this document.



Zero Trust Application Access -  SAML applications
This guide details the steps involved in adding SAML SSO supported third party applications into ZTAA. Since each Service Provider has different requirements to establish trust between the platform and itself, The process of adding the application will be detailed assuming that configuration has been done such that the Service Provider trusts the Instasafe IDP.

Navigate to the Identity Provider tab under Identity Management. Click on the ‘+’ to add a new service provider.
Select either the Office 365, Generic SP or the newly added one as your SP type.
On reaching the SAML Frontend , Click on Generate Certificate to generate the certificate and private key of the IDP.
Copy the certificate and private key and add them to the SP configuration and retrieve all the required information from the SP like ACS Url, Logout URL, SP Entity ID and add them into the IDP configuration on the portal.

Enable the access from the browser,desktop and mobile and click on next.
On the next panel, select the backend type as Local and click on Next and then submit the SP configuration.
To add applications, navigate to the Applications tab under Perimeter Management. Click on the ‘+’ icon to add a new application.Click on the application type required. Give the application an appropriate name and give its connection details like IP address and Port.On successful addition of the user, you will be given a notification in green. 
Navigate to the Identity Provider tab under Identity Management and navigate to the SP added in the previous steps. Go to edit mode by clicking on the view/edit toggle, Click on the ‘Add Application’ button to add the new application to the SP.
	Now users should be able to access the application from the application access section of the portal, the agent or the 4T Ready Client


Zero Trust Network Access

This guide details how to quickly set up a working ZTNA environment.
The following steps have to be performed by an admin user on the portal <companyname>.app.instasafe.io.

Create Users
To create users, Navigate to the Users tab under Identity Management.
 Click on the ‘+’ icon to add a new user. Details like the role of the user, contact details have to be provided on this screen. On successful addition of the user, you will be given a notification in green. 
You can verify the addition by refreshing the users page.

Create Applications
To add applications, navigate to the Applications tab under Perimeter Management.
Click on the ‘+’ icon to add a new application.Click on the application type required. Give the application an appropriate name and give its connection details like IP address (local) and Port.On successful addition of the user, you will be given a notification in green. 

Create Policies
To add policies, Navigate to Access Policies in menu bar on the left side
Click on the ‘+’ icon to add a new access policy. Give the policy an appropriate name and description and click on Next.
Click on the Add User button to add a user/users to the policy.
After adding user/users, Click on Next. 
This panel lets you add applications to the policy. This essentially means that all the users added to this policy will be able to access the applications that are added to this policy. Click on Add Application and add applications to the policy. And Click on next.
This panel allows the admin to configure specific rules and rulesets (a boolean combination of rules) that have to be met before being able to access the applications covered under this policy. Click on Next.
This panel allows you to configure OTP/Captcha access to the application in addition to the MFA during the initial login. Click on Next
This panel has the submit button to add this policy to the controller.On successful addition of the policy, you will be given a notification in green. 

Create a Gateway
The gateway is the relay point between the user and the application and point of access to all the applications secured by the ZTAA. Therefore, the security configuration of the gateway has to be impenetrable.

As an admin, Navigate to the Downloads page in the portal. Here you find the command to install the gateway on a linux machine
Prerequisites of the linux machine
A Linux machine which is publicly accessible has to be earmarked for the installation of the gateway software. 
This machine should be able to access all the applications that are to be accessed through the ZTAA network locally.
The machine should allow all inbound and outbound UDP traffic on port 443 publicly. It is strongly recommended that no ports other than 22 and 443 are opened publicly.
A user in the linux system with sudo privileges.
Once the prerequisites are met, SSH into the linux machine using the SSH client and authentication mechanism of your choice.
The installation script uses the curl command to download the installation script. Therefore, make sure that curl is installed on the system by runnin the following command
For Ubuntu and Debian systems
sudo apt install curl

For  RHEL, CentOS and Fedora distros
yum install curl


Once installed, we can run the gateway installation script. Before running the script, prepend the command in downloads with the keyword ‘sudo’. 
sudo /bin/bash -c "$(curl -fSsL https://storage.googleapis.com/4tr/start.sh)" 2>&1 | tee -a installationlogs.txt 

After some processing, you will be presented with the following menu

Enter 1 and press Enter. The following menu will appear

Enter 1 and press Enter. The installation process will begin.The next prompt will ask you to select the version of the gateway.
	
 Select the desired version by entering its corresponding number on the prompt and press Enter.
The installation process will start. It will download and install all the required packages. Once the installation is done, you will be presented with an access code as shown below.

Copy the access code here and head to the portal.
Navigate to the Gateways tab under Perimeter Management. Click on the ‘+’ to add a gateway. Select the option Private Gateway and click on Next.
On this panel, paste the access code that was generated by the gateway installation script and click on Next.
Finalize the installation by clicking on Authorize.
On successful addition of the gateway, you will be given a notification in green. 
The access code expires after a minute after it has been generated. Therefore, it is essential that the last few steps be performed quickly.
If the access code expires, follow all the steps until again step 8 where you are presented with an Install,Unistall screen. Uninstall the gateway and follow the gateway installation guide again to generate a new access code.


	
Add Applications to the Gateway
Now that the gateway has been added, the last part of the portal configuration is to add the applications to the gateway.
Navigate to the Gateways tab under Perimeter Management. Click on the gateway that has just been installed. The names of the gateways are the same as the hostname of the linux box they are hosted on.
On clicking the gateway, it expands to show more information and options. Here navigate to the Applications tab. Find View/Edit toggle on the topmost part of the screen and click to toggle between read and edit mode. 
Once in edit mode, Click on Add Applications, add the applications that are to be added to the gateway. Remember that the gateway can only access applications that are on the same VNC or network.
On successful addition of the application to the  gateway, you will be given a notification in green.

The ZTNA setup is complete. Users who were added in policies will be able access the applications through the agent. 
A guide for users to access applications can be found under the Getting Started section of this document.



Admin UI
The admin ui is the place where every administrative task available to the admin can be performed on the Zero Trust Platform.
The admin ui is common for both ZTAA  and ZTNA platforms. Admins can manage any number of these platforms with the help of the features provided by this UI.

The admin UI is driven by a View/Edit toggle to prevent accidental deletions. Modifications to existing entities can only be made in the edit mode.


Identity Management
This part of the console is to manage users, user groups, their authentication profiles and the level of their access on the platform.


The Dashboard shows an overview of the users and some useful information at a glance.

Users
A user in the Zero Trust platform allows any member of the organization to log onto the zero trust platform and access the resources that they are allowed access to by the company policies. The extent of access of a user on the Zero Trust platform is determined by the Role that has been  assigned.

There are two Roles that can be assigned to any user in the organization.
	User
	Admin

A user has read only privileges on the Zero Trust Network implying that they can only access the resources and cannot modify them in any way

Admin or Administrators have full control over the Zero Trust Platform and have all the privileges to create, modify or delete resources,users and infrastructure.


 
User Groups
Users are easily manageable when the size of the organization is small or medium but as the size of the organization keeps increasing, it poses an administrative challenge to the people maintaining the users on the platform. User Groups are a construct by which administrators can keep track of users as groups instead of individual users. This allows the administrators to perform actions on groups of users which saves them a lot of time and organizational effort. Additionally, Administrators can still grant users privileges apart from their group privileges.


Auth Profile
Users can be authenticated into the Zero Trust platform in the following ways
The primary authentication mode can be any of the following
Password
A username password can be used to authenticate a user into the platform. The admin need only add the user into the system, the password confirmation can be done from the side of the end user.
SAML
Instasafe Zero Trust can act as a SAML Service Provider to utilize any third party IDPs to log users into the system. 
Both IDP Initiated and SP Initiated modes of SAML Authentication are supported by the ZTAA platform
AD
Companies with existing Active Directories can add their servers to the platform and ZTAA will contact the AD server to authenticate the user’s credentials without the need to register the users into the platform. Further details about syncing the AD profiles can be found under the Directory Sync Profiles.
OAuth


In addition to the the primary authentication schemes, Instasafe offers several MultiFactor authentication schemes like
OTPs, Captcha and Security Questions can be configured.

The Session timeout i.e  the event occuring when a user does not perform any action on the platform during an interval and is subsequently logged out due to inactivity can be configured according to the company policy.

Identity Provider
For third party applications, Instasafe can act as an SAML Identity Provider and provide authenticated identities to a SAML SP to enable Single Sign On  with Instasafe’s user validation and device posture checks. This allows users to login into third party applications by a simple one click interface on the ZTAA Agent and Web Browser.
Directory Sync Profiles
The user database can be synced with the existing active directory of the company to assign privileges and resource access to various users/user groups. The user groups existing in the active directory can be imported into the  platform to provision resources without the need to create user groups on the platform separately.

Perimeter Management
Applications 

Administrators have control over the applications and their interaction with the end users. Administrators can restrict access to certain browser features like downloads, clipboard access, can record the screen for auditing purposes and set aliases for the applications.
The application groups supported by ZTAA are as follows
Web Applications
SSH
RDP
File Share
Windows Application
Android
IOS
Network Apps

Devices
Whenever a user connects to the Zero Trust Platform, the platform collects vital information about the user’s device to ensure the security posture of the device. The information collected can be reviewed and reused by the admin to make more exclusive rules to tighten the security of the network. The admin can also use this data to detect any rogue devices and actively block them right through the panel. Once blocked, the device cannot be used to access the platform when accessed from any interface on the computer. The collected data can be exported into a CSV for later review or piped into the data management system under configuration for further processing.

Gateways
As detailed in the architecture, The gateway is the policy enforcement point of the entire system. The gateway acts as the interface between the Internet and the protected network. Applications in the protected network can only be accessed through the gateway. Applications can be added and removed from the gateway dynamically.

Access Policies
Access policies are one of the most important security configurations in the ZTAA platform. It is through these policies that the ideas of micro segmentation and granular access control, both cornerstones of a Zero Trust network are  achieved on the platform. 

A basic access policy consists of a user and application in which case the user is allowed access to the application. By default, No user has access to an application when it is added to the gateway. An access rule including the user and the application has to be manually added to allow the user access to that application.

Instasafe’s Zero trust platform allows a lot more configuration with respect to the access policies. In addition to the basic rule allowing a user access to an application, Users groups can be added directly to the policy to access applications. Multiple users can be given access to multiple applications just by the means of a single access rule.

In addition to users and user groups, Each user can be validated against a set of preset boolean rules to allow access to an application. If the rule results in true, the user is allowed access to the application. Such rules can be configured on parameters such as

AntiVirus Updated Name,serial number,location,DateTime,OS Family,OS Main Version,OS Minor Version,OS Build Version,System Domain Name,Mac Address,AntiVirus Installed,AntiVirus Enabled,AntiVirus Updated,AntiVirus Installed Name,AntiVirus Enabled Name,AntiVirus Updated Name.

The platform can store reference values as datasets under data management to be used as a reference to compare the retrieved parameters with.
A complex check might look like the following image 

Here, every device’s MAC address is verified with a database of known MAC Addresses and OS Version is verified to be a known operating system. Such complex checks are important when dealing with highly critical applications which might warrant that extra layer of protection.

Audit

Audit provides an interface to monitor the entire Zero Trust Platform. All critical information about the users and their usage on the platform is logged and can be accessed from here.
A myriad of data points can be exported from the platform
Recent Active Users,Gateway Status,Username Lookups,User Logins,Authentication Logs,User Last Login,Never Loggedin,Application Access Request,Application Recordings,Session Logs,Events,T-OTP Status,Exports

The data can be displayed on the portal with options to sort and search  or can be exported to a CSV for ingestion into other data analysis platforms.

Configuration
Data Management
Datasets can be added into the platform to be used as reference value or values to compare existing device parameters against. 

Event Stream Profiles
The platform runs an event streaming server to capture events on the machines inside the protected network. The server is capable of capturing linux syslogs, FTP event logs, SFTP event logs etc. An event streaming client has to be installed on the end machine to push logs to the server. The portal then pulls these events from the event server and publishes them to the user.
VPN Profiles
ZTNA can be configured to allow access to a particular virtual address space when connected into the gateway. By default, a rule is added during the ZTNA setup but can be later configured as per the requirements of the users.


Application Access  

The session captures how an end user interacts with a ZTAA agent and accesses the application. The entire process is automated so that the end user has a simple one click interface to access the application.






Getting Started - End User
This section will be helpful for the users who are using the Instasafe Zero Trust platform for the first time to access an application, and want to learn their way around the platform.

Head to the <companyname>.app.instasafe.io and login with the username and password.
Your username and password will be provided by your IT administrator.
Once logged in with the credentials, You will be presented with the following screen
Navigate to the download icon (second icon) on the menu to the left. 
Download the agent for your operating system. 
Install the agent and login.
All the applications that you have access to will be shown on the screen
	
Clicking on any application will launch it within the agent.

Third party applications can be accessed through the web portal by logging to the portal as well.







Glossary
Diagrams
Documentation Diagrams
References
https://cloudsecurityalliance.org/artifacts/sdp-architecture-guide-v2/
