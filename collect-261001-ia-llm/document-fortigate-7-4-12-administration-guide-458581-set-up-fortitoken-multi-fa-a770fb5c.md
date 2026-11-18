Set up FortiToken multi-factor authentication
Set up FortiToken multi-factor authentication
This configuration adds multi-factor authentication (MFA) to the split tunnel configuration (SSL VPN split tunnel for remote user). It uses one of the two free mobile FortiTokens that is already installed on the FortiGate.
To configure MFA using the GUI:
- Configure a user and user group:
  - Go to User & Authentication > User Definition and edit local user sslvpnuser1.
  - Enable Two-factor Authentication.
  - For Authentication Type, click FortiToken and select one mobile Token from the list.
  - Enter the user's Email Address.
  - Enable Send Activation Code and select Email.
  - Click Next and click Submit.
- Activate the mobile token.When a FortiToken is added to user sslvpnuser1, an email is sent to the user's email address. Follow the instructions to install your FortiToken mobile application on your device and activate your token.
To configure MFA using the CLI:
- Configure a user and user group:config user local
    edit "sslvpnuser1"
        set type password
        set two-factor fortitoken
        set fortitoken <select mobile token for the option list>
        set email-to <user's email address>
        set passwd <user's password>
    next
end
config user group
    edit "sslvpngroup" 
        set member "sslvpnuser1"
    next 
end
- Activate the mobile token.When a FortiToken is added to user sslvpnuser1, an email is sent to the user's email address. Follow the instructions to install your FortiToken mobile application on your device and activate your token.