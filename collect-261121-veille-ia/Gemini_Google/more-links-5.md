---
id: collect-261121-veille-ia/gemini-google/more-links-5
title: "More links"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["user", "fortitoken", "mobile", "email", "token", "authentication", "configure", "sslvpnuser1"]
source: https://docs.fortinet.com/document/fortigate/latest/cookbook/458581/ssl-vpn-with-fortitoken-two-factor-authentication
source_anchor: ""
source_lines: [1, 35]
section: "Gemini_Google"
---
# Set up FortiToken multi-factor authentication

# Set up FortiToken multi-factor authentication

This configuration adds multi-factor authentication (MFA) to the split tunnel configuration (SSL VPN split tunnel for remote user). It uses one of the two free mobile FortiTokens that is already installed on the FortiGate.

###### To configure MFA using the GUI:

1. Configure the user:
  1. Go to *User & Device > User Definition* and edit local user*sslvpnuser1* .
  2. Enter the user's *Email Address* .
  3. Enable *Two-factor Authentication* and select one mobile*Token* from the list,
  4. Enable *Send Activation Code* and select*Email* .
  5. Click *Next* and click*Submit* .
2. Go to 
3. Activate the mobile token:
  1. When a FortiToken is added to user *sslvpnuser1* , an email is sent to the user's email address. Follow the instructions to install your FortiToken mobile application on your device and activate your token.
4. When a FortiToken is added to user 

###### To configure MFA using the CLI:

1. Configure the user:```
config user local
    edit "sslvpnuser1"
        set type password
        set two-factor fortitoken
        set fortitoken <select mobile token for the option list>
        set email-to <user's email address>
        set passwd <user's password>
    next
end
```
2. Activate the mobile token:
  1. When a FortiToken is added to user *sslvpnuser1* , an email is sent to the user's email address. Follow the instructions to install your FortiToken mobile application on your device and activate your token.
3. When a FortiToken is added to user
