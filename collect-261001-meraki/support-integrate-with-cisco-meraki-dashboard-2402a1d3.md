Integrate with Cisco Meraki Dashboard
Use JumpCloud SAML Single Sign On (SSO) to give your users convenient but secure access to all their web applications with a single set of credentials.
Read this article to learn how to integrate with Cisco Meraki Dashboard.
Prerequisites
- A JumpCloud administrator account
- JumpCloud SSO Package or higher or SSO add-on feature
- A Cisco Meraki Dashboard administrator account
Creating a new JumpCloud Application Integration
- Log in to the JumpCloud Admin Portal.
If your data is stored outside of the US, check which login URL you should be using depending on your region. If your organization uses LDAP, RADIUS, or requires firewall allow list configuration, the Fully Qualified Domain Names (FQDNs) will also be region specific. See JumpCloud Data Centers for the URLs, FQDNs, and IP addresses.
- Go to Access> SSO Applications.
- Click + Add New Application.
- You can also enter the name of the application in the Search field and select it.
- You can either select an application from the available list or select Custom Application, and click Next.
- Select the required options from the Select Options page and click Next. The Enter General Info page is displayed.
- On the Enter General Info page, you can customize the display label, description and how the application displays:
  - Description - add a description that users will see in their user portal
  - User Portal Image - choose Logoor Color Indicator
  - Show in User Portal - select to ensure the app is visible in the user portal
- Optionally, expand the Advanced Settings section and customize the IdP URL:
  - Enter a custom value to replace the default application name in the SSO IdP URL endpoint ( https://sso.jumpcloud.com/saml2/{custom_value} )
- Enter a custom value to replace the default application name in the SSO IdP URL endpoint ( 
The SSO IdP URL is not editable after the application is created. If you need to change this URL later, you must delete and recreate the connector.
- Click Save Application.
- Next, click:
  - Configure Application and go to the next section
  - Close to configure your new application at a later time
Users are implicitly denied access to applications. See Authorize Users to an SSO Application.
Linking the Application to AI Gateway
- The sections below walk through SSO configuration for this application. When that is complete, you can connect this application to JumpCloud AI Gateway so users can access its data from supported AI clients (for example, Cursor or ChatGPT).
- Register the MCP server under Access > AI Gateway and link it to this application. The server appears under Servers in AI Gateway. When you open the application from Access > SSO Applications, the same server appears on the application's AI Gateway tab.
- Not all applications support MCP. Confirm support with the application vendor or see Configure AI Gateway Integrations to learn more about supported integrations.
Configuring the SSO Integration
To navigate to your JumpCloud SSO connector
- Log in to the JumpCloud Admin Portal.
If your data is stored outside of the US, check which login URL you should be using depending on your region. If your organization uses LDAP, RADIUS, or requires firewall allow list configuration, the Fully Qualified Domain Names (FQDNs) will also be region specific. See JumpCloud Data Centers for the URLs, FQDNs, and IP addresses.
- 
Go to Access > SSO Applications.
- 
Create a new application or select it from the Configured Applications list.
- 
Select the SSO tab.
Download the certificate
- If you closed the application, find it in the Configured Applications list and click anywhere in the row to reopen its configuration window.
- Click Actions > Download Certificate.
The certificate.pem will download to your local Downloads folder.
To configure Cisco Meraki Dashboard
- Sign in to Meraki Dashboard as an administrator.
- Navigate to Organization > Settings.
- In the SAML Configuration section, select SAML SSO enabled from the SAML SSO dropdown menu, then click Add a SAML IdP.
- Enter the following information:
  - X.509 cert SHA1 fingerprint - paste the THUMBPRINT value. See Determining the Sha1 Fingerprint to determine the THUMBPRINT from the certificate.pem file downloaded in the previous section
  - SLO logout URL (optional) - enter https://console.jumpcloud.com/userconsole/
- Click Save.
- On the Organization settings page, make a copy of the Consumer URL value.
- Navigate to Organization > Administrators.
- Click Add SAML role.
- Enter a Role name, and select the appropriate Organization access and privileges, then click Create role.
- Click Save changes.
To configure JumpCloud
- Back in the SSO tab of your JumpCloud Cisco Meraki connector, paste the Meraki Consumer URL to the Default URL field in the ACS URLs section.
- Optionally, configure:
MFA Claims
The Authentication Methods References (AMR) is automatically included in the SAML assertion by default. No additional configuration is required to enable this.
Complete the MFA Claim Configuration to define how the authentication context is sent in the SAML assertion.
- Under Auth Context, choose one of the following options based on your SP's requirements:
  - Send a single value for all successful MFA factors - select if the Service Provider accepts a generic confirmation for any MFA login. Enter the single URL or URN they accept
  - Send specific factors - select this option to map individual JumpCloud MFA methods to distinct values. In the Factor Mapping table, add each MFA factor enabled in your organization and enter the corresponding value required by the Service Provider
  - Send single value and specific factors - select to send both a generic identifier and specific factor details in the assertion
Refer to your Service Provider's documentation to determine the specific URN or URL values required (e.g., Salesforce Session Security Levels). The values entered in this configuration must exactly match what the Service Provider expects.
| MFA Factor | Service Provider | Notes | 
|---|---|---|
| Password | Reference your Service Providers's documentation for the values they expect for each factor |  | 
| TOTP |  |  | 
| WebAuthN |  |  | 
| Push Notification |  | JumpCloud Protect or other authenticator application | 
| Duo Security |  |  | 
| Device Trust |  |  | 
| Device Trust + User Verification |  | JumpCloud Go (requires explicit configuration - see the next table) | 
| API Key |  |  | 
| External Identity Provider |  |  | 
| MFA Method | MFA Value in AMR Claim | 
|---|---|
| apikey | swk | 
| duo | mfa | 
| pwd | pwd | 
| totp | otp | 
| unk |  | 
| wan | hwk | 
| push | mfa | 
| uv |  | 
| durt | hwk | 
| durt_uv | hwk | 
| ext_idp |  | 
Learn more about MFA Claims.
Attributes
Configure User Attributes to be sent to the SP in assertions. User attributes are unique to each user. You can include attributes for standard user detail attributes or for custom attributes. For example, you can include standard attributes for users’ employee ID and department, or you can include a custom attribute for users’ application ID. Standard attributes are configured in the User Panel Details tab's User Information andEmployee Information sections.
Unlike user attributes, a Constant Attribute can be sent for every user in a specific group or application profile.
If required attributes are present, they are not editable.
- Under User Attributes, click add attribute:
  - Service Provider Attribute Name - enter the service provider’s name for the attribute
  - JumpCloud Attribute Name - select the corresponding attribute from the drop down list
- Repeat these steps for any desired user or custom attributes.
- Under Constant Attributes, click add attribute:
  - Service Provider Attribute Name - enter the service provider’s name for the attribute
  - Value - enter the corresponding attribute in JumpCloud
- Optionally, if groups are supported, select Include Group Attribute.
- In the Attributes section, confirm the JumpCloud Attribute Name matches the attribute used in Meraki Dashboard.
This will be either Email or Username.
- Under Service Provider Attribute Name, enter a value for the role attribute defined in the previous section.
A role attribute must be passed in the SAML token/assertion, specifically 'https://dashboard.meraki.com/saml/attributes/role'. This must match one of the Roles defined on the Organization > Administrators page. Dashboard will only accept one role attribute. If multiple roles or group memberships are provided, the first attribute matched will be used. If 'MemberOf' and 'role' attributes are both specified, 'MemberOf' will be prioritized.
- Under Constant Attributes, select OFF if you do not want to use JIT.
- Click Save.
Using JIT Provisioning
Additional attributes are required to use JIT provisioning. JIT required attributes are prepopulated and are on by default to enable JIT provisioning. You can’t edit the JIT required service provider attributes. You can customize the JumpCloud attribute name and the constant value for JIT required attributes. Toggle off the attributes to opt out of sending the attributes in the SAML assertion.
To complete the provisioning process
- Authorize a user’s access to the application in JumpCloud.
- Have the user log in to the application using SSO. The SAML assertion passes from JumpCloud to the service provider, and gives the service provider the information it needs to create the user account.
Authorizing SSO Application Access
Users are implicitly denied access to SSO Applications. After you connect an application to JumpCloud, you need to authorize user access to that application. You can authorize user access from the Applications, Users List or User Groups page.
To authorize user access from the SSO Application’s page
- Log in to the JumpCloud Admin Portal.
If your data is stored outside of the US, check which login URL you should be using depending on your region. If your organization uses LDAP, RADIUS, or requires firewall allow list configuration, the Fully Qualified Domain Names (FQDNs) will also be region specific. See JumpCloud Data Centers for the URLs, FQDNs, and IP addresses.
- Go to Access > SSO Applications, then select the application to which you want to authorize user access.
- Select the User Groups tab. If you need to create a new group of users, see Get Started: User Groups.
- Select the check box next to the desired group of users to which you want to give access.
- Click Save.
To learn how to authorize user access from the Users or User Groups pages, see Authorize Users to an SSO Application.
Validating SSO user authentication workflow(s)
Check your SP's documentation to ensure that both workflows are supported.
IdP-initiated user workflow
- Access the JumpCloud User Console
- Go to Applications and click an application tile to launch it
- JumpCloud asserts the user's identity to the SP and is authenticated without the user having to log in to the application
SP-initiated user workflow
- Go to the SP application login - generally, there is either a special link or an adaptive username field that detects the user is authenticated through SSO
This varies by SP.
- Login redirects the user to JumpCloud where the user enters their JumpCloud credentials
- After the user is logged in successfully, they are redirected back to the SP and automatically logged in
See Additional User Experience Considerations when setting up JumpCloud SSO.
Removing the SSO Integration
These are steps for removing the integration in JumpCloud. Consult your SP's documentation for any additional steps needed (like disabling "mandatory SSO login" settings) to remove the integration in the SP. Failure to remove the integration successfully for both the SP and JumpCloud may result in users, including admins, losing access to the application.
If your data is stored outside of the US, check which login URL you should be using depending on your region. If your organization uses LDAP, RADIUS, or requires firewall allow list configuration, the Fully Qualified Domain Names (FQDNs) will also be region specific. See JumpCloud Data Centers for the URLs, FQDNs, and IP addresses.
To deactivate the SSO Integration
- 
Log in to the JumpCloud Admin Portal.
- 
Go to Access > SSO Applications.
- 
Search for the application that you’d like to deactivate and click to open its details panel.
- 
Select the SSOtab.
- 
Scroll to the bottom of the configuration.
- 
Click Deactivate SSO.
- 
Click Save.
- 
If successful, you will receive a confirmation message.
To delete the application
- 
Log in to the JumpCloud Admin Portal.
- 
Go to Access > SSO Applications.
- 
Search for the application that you’d like to delete.
- 
Check the box next to the application to select it.
- 
Click Delete.
- 
Enter the number of the applications you are deleting
- 
Click Delete Application.
- 
If successful, you will see an application deletion confirmation notification.
Was this information helpful?