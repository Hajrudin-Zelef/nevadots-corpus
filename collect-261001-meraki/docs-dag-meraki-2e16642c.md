Duo Protection for Meraki Dashboard with Duo Access Gateway
Last updated:
Duo Access Gateway reaches Last Day of Support on October 26, 2023 for Duo Essentials, Advantage, and Premier customers. As of that date Duo Support may only assist with the migration of existing Duo Access Gateway applications to Duo Single Sign-On. Customers may not create new DAG applications after May 19, 2022. Please see the Guide to Duo Access Gateway end of life for more details.
Use the Duo Single Sign-on for Meraki Dashboard application to protect Meraki Dashboard with Duo Single Sign-On, our cloud-hosted identity provider featuring Duo Central and the Duo Universal Prompt.
Duo Federal customers may still use Duo Access Gateway for SAML applications after October 26, 2023.
Overview
As business applications move from on-premises to cloud hosted solutions, users experience password fatigue due to disparate logons for different applications. Single sign-on (SSO) technologies seek to unify identities across systems and reduce the number of different credentials a user has to remember or input to gain access to resources.
While SSO is convenient for users, it presents new security challenges. If a user's primary password is compromised, attackers may be able to gain access to multiple resources. In addition, as sensitive information makes its way to cloud-hosted services it is even more important to secure access by implementing two-factor authentication and zero-trust policies.
Duo Access Gateway
Duo Access Gateway (DAG), our on-premises SSO product, layers Duo's strong authentication and flexible policy engine on top of Meraki logins using the Security Assertion Markup Language (SAML) 2.0 authentication standard. Duo Access Gateway acts as an identity provider (IdP), authenticating your users using existing on-premises Active Directory (AD) credentials and prompting for two-factor authentication before permitting access to Meraki.
Duo Access Gateway is included in the Duo Premier, Duo Advantage, and Duo Essentials plans, which also include the ability to define policies that enforce unique controls for each individual SSO application. For example, you can require that Salesforce users complete two-factor authentication at every login, but only once every seven days when accessing Meraki. Duo checks the user, device, and network against an application's policy before allowing access to the application.
Deploy or Update Duo Access Gateway
- 
Install Duo Access Gateway on a server in your DMZ. Follow our instructions for deploying the server, configuring DAG settings, and adding your primary authentication source. 
- 
Include the AD attributes mail,distinguishedName in the "Attributes" field when configuring the Active Directory authentication source in the DAG admin console. You must use Active Directory as your authentication source; other DAG authentication sources do not support Meraki logins. If you've already configured the attributes list for another cloud service provider, append the additional attributes not already present to the list, separated by a comma.
- 
After completing the initial DAG configuration steps, click Applications on the left side of the Duo Access Gateway admin console.
- 
Scroll down the Applications page to the Metadata section. This is the information you need to provide to Meraki when configuring SSO.
Enable Meraki SSO
Add Duo SAML Provider
Add the Duo Access Gateway as a new single sign-on provider for Meraki.
- 
Log in to the Meraki as an administrative user and navigate to Organization → Configure → Settings.
- 
Scroll down until you find SAML Configuration.
- 
In the SAML SSO drop-down select SAML SSO enabled the setting will automatically expand.
- 
Make note of the Consumer URL — you will need this information later. Example: https://n000.meraki.com/saml/login/xx000x
- 
Copy the SHA-1 Fingerprint from the Duo Access Gateway admin console Metadata display and paste it into the Meraki X509 cert SHA1 fingerprint field.
- 
Copy the Logout URL information from the Duo Access Gateway admin console Metadata display and paste it into the Meraki SLO logout URL (optional) field. Example: https://yourserver.example.com/dag/saml2/idp/SingleLogoutService.php
- 
After you've entered all the required information scroll down and click Save Changes.
Create Meraki Role for SAML
Next, create a SAML role in Meraki that uses the SAML provider you just created, and grant Meraki service and resource access to that role.
- 
In the Meraki console navigate to Organization → Configure → Administrators.
- 
Click the Add SAML role button.
- 
Enter a role name in the Role field. This role name must begin with the Group Prefix you'll define below and have a corresponding, identically named group in Active Directory. Add organization access and permissions to this role as needed. Click the Create admin button when finished.
- 
Click Save changes on the "Administrators" page.
Learn more about Meraki SSO at Meraki Support.
Create the Meraki Application in Duo
- 
Log in to the Duo Admin Panel and navigate to Applications → Application Catalog.
- 
Locate the entry for Meraki with the "DAG" label in the catalog. Click the + Add button to start configuring Meraki. See Protecting Applications for more information about protecting applications with Duo and additional application options.
- 
No active Duo users can log in to new applications until you grant access. Update the User access setting to grant access to this application to users in selected Duo groups, or to all users. Learn more about user access to applications. If you do not change this setting now, be sure to update it so that your test user has access before you test your setup. This setting only applies to users who exist in Duo with "Active" status. This does not affect application access for existing users with "Bypass" status, existing users for whom the effective Authentication Policy for the application specifies "Bypass 2FA" or "Skip MFA", or users who do not exist in Duo when the effective New User Policy for the application allows access to users unknown to Duo without MFA.
- 
Enter the Consumer URL from Meraki SAML Configuration settings page. Example: https://n000.meraki.com/saml/login/xx000x
- 
Group Prefix allows you to define a custom prefix that all Active Directory groups and Meraki roles will need to match. Example: If you use the default prefix DAG-Meraki- your groups in Active Directory and roles in Meraki should be named something similar to DAG-Meraki-Admins.
- 
Meraki uses the Mail attribute when authenticating. We've mapped Mail attribute to DAG supported authentication source attributes as follows: Duo Attribute Active Directory Mail attribute mail If you are using a non-standard email attribute for your authentication source, check the Custom attributes box and enter the name of the attribute you wish to use instead.
- 
Click Save Configuration to generate a downloadable configuration file.
- 
You can adjust additional settings for your new SAML application at this time — like changing the application's name from the default value, enabling self-service, or assigning a group policy — or come back and change the application's policies and settings after you finish SSO setup. If you do update any settings, click the Save button at the bottom of the page when done.
- 
Click the Download your configuration file link to obtain the Meraki application settings (as a JSON file). Important: This file contains information that uniquely identifies this application to Duo. Secure this file as you would any other sensitive or password information. Don't share it with unauthorized individuals or email it to anyone under any circumstances!
Duo Universal Prompt
The Duo Universal Prompt provides a simplified and accessible Duo login experience for web-based applications, offering a redesigned visual interface with security and usability enhancements.
| Universal Prompt | Traditional Prompt | 
Read the Universal Prompt Update Guide for more information about the update process and the new login experience for users.
Duo Access Gateway Universal Prompt support is available to Duo Federal customers only starting with version 2.0.0. Activating Universal Prompt for DAG applications requires the following steps:
- Enable Use frameless in the local DAG server admin console on the **General** page.
- Complete the federation steps in this document for your SAML app.
- Log in as an end user and complete Duo authentication to the SAML application now federated with your Duo Access Gateway server. This first authentication shows the traditional Duo prompt in a redirect instead of an iframe.
- You then activate the Universal Prompt for all users of that specific DAG SAML application.
The "Universal Prompt" section of your DAG SAML app shows the status as "Update Required" when you first create it. The status will change after you log in to the application and complete Duo authentication to the SAML application now federated with your Duo Access Gateway server.
Add the Meraki Application to Duo Access Gateway
Before you do this, verify that you updated the "Attributes" list for your Duo Access Gateway authentication source as specified here.
- 
Return to the Applications page of the DAG admin console session.
- 
Click the Choose File button in the "Add Application" section of the page and locate the Meraki SAML application JSON file you downloaded from the Duo Admin Panel earlier. Click the Upload button after selecting the JSON configuration file.
- 
The Meraki SAML application is added. Copy the Login URL for the Meraki application (it looks like https://yourserver.example.com/dag/saml2/idp/SSOService.php?spentityid=https://dashboard.meraki.com). That is the URL you will use to log in to the Meraki dashboard via DAG.
Verify SSO
You can log in to Meraki with IdP-initiated SSO using the Login URL provided by the DAG server when you created the Meraki application in the DAG admin console (https://yourserver.example.com/dag/saml2/idp/SSOService.php?spentityid=https://dashboard.meraki.com).
Enter your primary directory username and password on the Duo Access Gateway login page, approve Duo two-factor authentication, and get redirected to the Meraki site after authenticating.
Congratulations! Your Meraki users now authenticate using Duo Access Gateway.
Activate Universal Prompt
Once you authenticate to your newly-federated SAML application, the "Universal Prompt" section of the application's details page in the Admin Panel reflects this status as "Ready to activate", with these activation control options:
- Show traditional prompt: (Default) Your users experience Duo's traditional prompt via redirect when logging in to this application.
- Show new Universal Prompt: Your users experience the Universal Prompt via redirect when logging in to this application.
Enable the Universal Prompt experience by selecting Show new Universal Prompt, and then scrolling to the bottom of the page to click Save.
Once you activate the Universal Prompt, the application's Universal Prompt status shows "Activation Complete" here and on the Universal Prompt Update Progress report.
The next time your users log in to this application, they will see the new Universal Prompt experience instead of the traditional Duo prompt.
If you plan to permit use of WebAuthn authentication methods (security keys, U2F tokens, or Touch ID) in the traditional Duo Prompt, Duo recommends configuring allowed hostnames for this application and any others that show the inline Duo Prompt before onboarding your end-users.
The Duo Universal Prompt has built-in protection from unauthorized domains so this setting does not apply.
Add Ownership and Risk Information
Go to the "Ownership and Risk" section of the application's page in the Duo Admin Panel to assign application owners and classify the application's risk level. Cisco Identity Intelligence automatically imports this information to populate relevant fields for Duo Advantage and Premier customers. Duo Essentials and Duo Federal plans exclude Cisco Identity Intelligence features.
You may set any of the following:
- Technical Owner: Search for and assign Duo users responsible for technical configuration and maintenance of this application.
- Business Owner: Search for and assign Duo users responsible for business decisions and access approvals related to this application.
- Application Sensitivity: Select this application's risk level from the drop-down list. Default: Not Set.
- Compliance Requirements: Select any applicable regulatory frameworks for this application (SOX (Sarbanes-Oxley), HIPAA, PCI-DSS, etc.) from the list.
Scroll to the bottom of the page and click Save to apply your changes.
Grant Access to Users
If you did not already grant user access to the Duo users you want to use this application be sure to do that before inviting or requiring them to log in with Duo.
Meraki does not support SP-initiated SSO login at this time.
Microsoft AD FS
Microsoft's Active Directory Federation Services (AD FS) is a popular choice for SSO because it easily integrates with the AD identity store many organizations already have deployed. Duo's support for cloud applications and SSO drops in to an existing AD FS installation to provide secondary authentication after a user passes primary authentication (successful Active Directory logon).
If you don't already have AD federation running the first step is to install and configure Microsoft AD FS in your organization. Deployment Guides for AD FS versions 2.1, and 3.0/4.0 are available from Microsoft.
Once your AD FS services are up and running, the second step is to configure the SSO partnership between your AD FS service and the external cloud resource, in this case Meraki. Learn more about configuring Meraki SSO with AD FS at the Meraki Support site.
After you have successfully configured and tested AD FS SSO login to Meraki using your AD domain credentials, you can then install the Duo AD FS integration. AD FS protection is included with Duo Essentials, Duo Advantage, and Duo Premier plans.
With the Duo integration for AD FS installed, users pass primary authentication to the AD FS service as usual. Once primary authentication succeeds, users are forwarded to the Duo service for secondary authentication. After approving logon using one of Duo's authentication methods, the user is fully logged in to Meraki.
Other Identity Partners
Using a third-party SSO provider for cloud application access? Duo partners with leading cloud SSO providers like Okta and OneLogin to secure access with our strong and flexible authentication platform.
You can also use Duo two-factor authentication with CAS and Shibboleth on-premises IdPs.
Troubleshooting
Need some help? Try searching our Knowledge Base articles or Community discussions. For further assistance, contact Support.