---
id: collect-261121-veille-ia/fortinet/sheynshield-fortinet-fortigate-cheatsheet-user-authentication-radius-checklist-md-at-172fb9490610bed
title: "sheynshield/Fortinet/FortiGate/CheatSheet/User-Authentication/RADIUS-Checklist.md at 172fb9490610bedf2470836990d0ff5347fa74a0 · Shayan-heydarikhah/sheynshield"
domain: fortinet
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["radius", "authentication", "fortigate", "accounting", "group", "sheynshield", "server", "port"]
source: https://github.com/shayan-heydarikhah/sheynshield/blob/HEAD/Fortinet/FortiGate/CheatSheet/User-Authentication/RADIUS-Checklist.md
source_anchor: ""
source_lines: [1, 1070]
section: "Copilot_Microsoft"
---
**SheynShield | Security & Design Knowledge Base**
**FortiGate / FortiOS | NSE4 → NSE7**

Practical **RADIUS Server checklist** covering **RADIUS authentication, NPS, Active Directory, VSA, AVP, RSSO, RADIUS Accounting, CoA, group mapping, dynamic shaping, wireless attributes, troubleshooting, and FortiGate CLI**.


**RADIUS — Remote Authentication Dial-In User Service** provides centralized:

- Authentication
- Authorization
- Accounting

Typical FortiGate architecture:

```
                    ┌─────────────────────┐
                    │ Active Directory    │
                    │       / LDAP        │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │        NPS          │
                    │      RADIUS         │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
              UDP 1812                   UDP 1813
            Authentication               Accounting
                 │                           │
                 └─────────────┬─────────────┘
                               ▼
                    ┌─────────────────────┐
                    │     FortiGate       │
                    │                     │
                    │ Authentication      │
                    │ Group Mapping       │
                    │ RSSO                │
                    │ Dynamic Shaping      │
                    └──────────┬──────────┘
                               │
                               ▼
                       Firewall Policy
                               │
                               ▼
                            Access
```
- RADIUS/NPS server is available
- Active Directory integration is working
- FortiGate is defined as a RADIUS client/NAS
- Shared secret is configured consistently
- Authentication port is reachable
- Accounting port is reachable when required
- NPS policies are configured
- FortiGate user group is configured
- Firewall policy references the correct group

| Function | UDP Port | 
|---|---|
| Authentication / Authorization | `1812` | 
| Accounting | `1813` | 
| Legacy Authentication | `1645` | 
| Legacy Accounting | `1646` | 

- UDP/1812 allowed from FortiGate to RADIUS
- UDP/1813 allowed when accounting is required
- Legacy ports are used only when required
- Routing between FortiGate and RADIUS is correct
- Intermediate firewalls permit required traffic

```
1812 → Authentication
1813 → Accounting
1645 → Legacy Authentication
1646 → Legacy Accounting
```
**NSE Gotcha:** Authentication can work perfectly while accounting-dependent features fail because UDP/1813 is unavailable.

⚠️ 

```
User & Authentication
└── RADIUS Servers
    └── Create New
```
Example:

```
Name:              rad-1
Server IP:         192.168.20.200
Authentication:    MSCHAPv2
NAS IP:            192.168.20.254
Auth Port:         1812
Accounting Port:   1813
```
- RADIUS object created
- Correct server IP configured
- Correct authentication method selected
- Shared secret configured
- NAS IP configured correctly when required
- Authentication port verified
- Accounting port verified
- NPS recognizes FortiGate as a RADIUS client

```
config user radius
    edit "rad-1"
        set server "192.168.20.200"
        set secret <RADIUS_SECRET>
    next
end
```
🔐 Never commit a real RADIUS shared secret to GitHub.


Create a FortiGate remote authentication group:

```
User & Authentication
└── User Groups
    └── Create New
```
Example:

```
Group Name:      rad-g-1
Remote Server:   rad-1
```
- RADIUS server selected
- Correct remote group configuration
- Returned RADIUS attributes understood
- Group referenced by firewall policy
- NPS group membership verified

```
Active Directory
      ↓
AD Group
      ↓
NPS Policy
      ↓
RADIUS Response
      ↓
FortiGate Group
      ↓
Firewall Policy
      ↓
Access
```
💡 **Critical troubleshooting rule:**
`Authentication = SUCCESS` does not necessarily mean `Firewall Policy Match = SUCCESS`.


Example:

```
config user radius
    edit "rad-1"
        set password-encoding auto
        set username-case-sensitive enable
    next
end
```
- Password encoding matches the deployment
- Username format is consistent
- Username case behavior is understood
- AD username format is verified
- NPS username handling is verified

```
User01
user01
USER01
```
Depending on the FortiOS/RADIUS/backend behavior, these may not be treated identically.

```
FortiGate
   ↓
RADIUS
   ↓
NPS
   ↓
Active Directory
```
Keep username normalization and case handling consistent across the entire authentication chain.

Standard RADIUS attributes follow:

```
Attribute = Value
```
Common attributes:

| Attribute | Code | Purpose | 
|---|---|---|
| User-Name | `1` | Username | 
| NAS-IP-Address | `4` | NAS address | 
| Framed-IP-Address | `8` | Client IP | 
| Class | `25` | Session/accounting correlation | 
| Vendor-Specific | `26` | Vendor attribute container | 
| NAS-Identifier | `32` | NAS identifier | 
| Acct-Input-Octets | `42` | Received octets | 
| Acct-Output-Octets | `43` | Transmitted octets | 
| Acct-Session-Id | `44` | Session identifier | 
| Event-Timestamp | `55` | Event timestamp | 

- Attribute is supported
- Attribute code is correct
- Attribute value is correct
- FortiGate interprets the attribute as expected
- NPS policy returns the required attribute

**VSA = Vendor-Specific Attribute**

Fortinet-specific RADIUS information can be carried using the RADIUS Vendor-Specific Attribute mechanism.

```
RADIUS Attribute 26
        │
        ▼
Fortinet VSA
        │
        ├── Group
        ├── Client IP
        ├── VDOM
        ├── IPv6 information
        ├── Interface
        └── Access Profile
```
```
Vendor:
Fortinet
Vendor-ID:
12356
```
| VSA | Attribute | Purpose | 
|---|---|---|
| `1` | `fortinet-group-name` | Remote group mapping | 
| `2` | `fortinet-client-ip-address` | Client IP | 
| `3` | `fortinet-vdom-name` | VDOM information | 
| `4` | `fortinet-client-ipv6-address` | IPv6 client information | 
| `5` | `fortinet-interface-name` | Interface information | 
| `6` | `fortinet-access-profile` | Administrative access profile | 

- Fortinet vendor ID is correct
- Correct VSA is returned by RADIUS
- NPS vendor-specific attribute is configured correctly
- Attribute value matches FortiGate expectations
- Returned VSA is visible during troubleshooting
- Group mapping uses the correct returned value

A common enterprise design is:

```
Active Directory Group
          ↓
      NPS Policy
          ↓
    RADIUS Response
          ↓
    Fortinet VSA
          ↓
     FortiGate Group
          ↓
    Firewall Policy
```
Example:

```
AD User:
    alice
AD Group:
    IT
NPS:
    Match IT group
RADIUS:
    Fortinet-Group-Name = IT
FortiGate:
    Remote Group = IT
```
- User belongs to expected AD group
- NPS policy matches the user
- NPS policy returns expected attribute
- Fortinet VSA is correctly configured
- Returned group name is correct
- FortiGate remote group matches returned value
- Firewall policy references the expected group

If:

```
Authentication = SUCCESS
Policy Match    = FAILURE
```
check:

```
NPS Policy
   ↓
RADIUS Attributes
   ↓
VSA / Filter-Id
   ↓
FortiGate Group
   ↓
Firewall Policy
```
before assuming the RADIUS password authentication itself is broken.

RADIUS Accounting provides session information such as:

- Session start
- Interim updates
- Session stop
- User/session correlation
- Usage information
- RSSO information
- Dynamic policy context

```
FortiGate
   │
   │ Accounting-Request
   ▼
RADIUS Server
   │
   ├── Start
   ├── Interim-Update
   └── Stop
```
- UDP/1813 reachable
- Accounting server configured
- Accounting secret matches
- Start packets received
- Interim updates configured where required
- Stop packets received
- Session IDs are consistent
- Accounting data is visible on the RADIUS server

**RSSO = RADIUS Single Sign-On**

RSSO allows FortiGate to obtain user/session information associated with RADIUS authentication and use that identity for policy enforcement.

```
Security Fabric
└── External Connectors
    └── RADIUS Single Sign-On
```
Example:

```
Name:
    rsso-agent
```
Then create/use a corresponding FortiGate user group.

```
User & Authentication
└── User Groups
    └── rsso-f
```
```
User
  ↓
Network Access Device
  ↓
RADIUS Authentication
  ↓
RADIUS Server
  ↓
Accounting / Session Information
  ↓
FortiGate RSSO
  ↓
User ↔ IP Association
  ↓
FortiGate User Group
  ↓
Firewall Policy
```
- RSSO connector configured
- RADIUS authentication works
- RADIUS accounting works
- User/session information reaches FortiGate
- User-to-IP association is learned
- RSSO group is configured
- Firewall policy references RSSO identity

| Feature | RSSO | FSSO Polling | FSSO Collector | 
|---|---|---|---|
| Primary source | RADIUS | AD | AD/DC | 
| Authentication awareness | RADIUS-based | Polling | Logon events | 
| Main identity source | RADIUS session | AD | AD | 
| Best fit | RADIUS environments | Simple AD | Enterprise AD | 
| Session awareness | RADIUS-based | AD polling | AD logon events | 

```
RADIUS infrastructure
        ↓
       RSSO
Simple AD environment
        ↓
   FSSO Polling
Enterprise AD / multiple DCs
        ↓
 FSSO Collector / DC Agent
```
**CoA = Change of Authorization**

CoA allows the RADIUS infrastructure to request changes to an existing authenticated session.

```
RADIUS
   │
   │ CoA
   ▼
FortiGate
   │
   ├── Update authorization
   ├── Update session
   └── Apply supported attributes
```
- Dynamic authorization
- User policy changes
- Bandwidth changes
- Wireless environments
- Session updates
- Post-authentication authorization

- CoA supported by target FortiOS release
- RADIUS server supports required CoA behavior
- FortiGate CoA configuration verified
- Required network path is available
- Shared secret/configuration is correct
- Session is eligible for the requested update

RADIUS attributes can participate in dynamic traffic-shaping designs.

Example:

```
config user radius
    edit "rad-1"
        set radius-coa enable
        set radius-all-server enable
    next
end
```
⚠️ Exact CLI availability and behavior can vary by FortiOS release. Validate commands against the target FortiOS version.

Example:

```
config firewall policy
    edit 1
        set dynamic-shaping enable
    next
end
```
```
RADIUS User
     ↓
Identity
     ↓
RADIUS Attributes
     ↓
FortiGate Session
     ↓
Firewall Policy
     ↓
Dynamic Shaper
     ↓
User Traffic
```
Example:

```
User A → 20 Mbps
User B → 10 Mbps
User C →  5 Mbps
```
- User identity is correctly learned
- Required RADIUS attributes are returned
- Accounting is working when required
- Policy supports dynamic shaping
- Dynamic shaping is enabled
- User/session is associated correctly
- Shaper statistics confirm enforcement

Wireless environments may use additional attributes to carry information related to:

- SSID
- Wireless controller/device
- Wireless termination point
- Association time

Fortinet-specific wireless information may include:

| Code | Concept | 
|---|---|
| `7` | Fortinet SSID | 
| `23` | Wireless device/controller | 
| `24` | Wireless termination point | 
| `25` | Association time | 

- RADIUS authentication works
- Accounting is received
- Wireless client association is correct
- AP/controller information is correct
- Session state is synchronized
- Roaming behavior is validated
- Dynamic shaping is verified

Termination behavior affects how session state changes are handled.

Conceptual values:

| Value | Concept | 
|---|---|
| `0` | Default/normal behavior | 
| `1` | RADIUS request/session verification behavior | 
| `2` | Resource/session management and cleanup | 

**FortiOS-version and deployment dependent**. Validate the exact behavior against the target release and RADIUS integration.

⚠️ Treat these values as

- Required termination behavior is identified
- FortiOS version is verified
- RADIUS server behavior is verified
- Wireless session behavior is tested
- Stale-session scenarios are tested

RADIUS can return attributes used by FortiGate for identity/group decisions.

Example:

```
RADIUS
   │
   └── Filter-Id = IT
             │
             ▼
      FortiGate Group
             │
             ▼
      Firewall Policy
```
- Returned attribute is expected
- Attribute value is correct
- FortiGate group expects the same value
- NPS policy returns the attribute
- Firewall policy references the correct group

```
AD Group
   ↓
NPS Policy
   ↓
RADIUS Attribute
   ↓
FortiGate Group
   ↓
Firewall Policy
```
Any mismatch can cause policy selection failure.

RSA SecurID provides OTP-based authentication.

Conceptual architecture:

```
User
 │
 ├── Username
 └── OTP
      │
      ▼
FortiGate
      │
      ▼
RADIUS
      │
      ▼
RSA Authentication Infrastructure
```
`diagnose test authserver radius <server> <username> <password>`
- OTP integration is validated
- RADIUS authentication works
- Token/OTP policy is correct
- No OTP secrets are stored in documentation
- Production credentials are never committed to GitHub

🔐 **Never publish real passwords, OTP seeds, RADIUS secrets, certificates containing private keys, or production credentials.**


- FortiGate can reach RADIUS server
- Correct routing exists
- UDP/1812 is allowed
- UDP/1813 is allowed when required
- No intermediate firewall blocks RADIUS

- RADIUS server is online
- FortiGate is configured as a RADIUS client
- Shared secret matches
- NAS IP is correct
- Authentication method matches
- NPS policy is enabled

Run:

`diagnose test authserver radius <server> <username> <password>`
Validate:

- Authentication request reaches server
- Server returns Access-Accept
- No Access-Reject
- No unexpected Access-Challenge
- Authentication method is compatible

If authentication succeeds:

```
Authentication
      ↓
      ?
FortiGate Group
```
Check:

- NPS policy
- AD group membership
- VSA
- Filter-Id
- Returned attributes
- FortiGate remote group
- Firewall policy

If RSSO fails:

- Authentication works
- Accounting works
- UDP/1813 is reachable
- RSSO connector is enabled
- Session information is received
- User ↔ IP association exists
- RSSO group is correct

Check:

`diagnose firewall shaper dynamic-shaper stats`
Then:

`diagnose firewall shaper dynamic-shaper list`
Specific client:

`diagnose firewall shaper dynamic-shaper list ip 192.168.20.20`
Validate:

- Correct user identity
- Correct policy
- Required RADIUS attributes
- Correct shaper
- Active session
- Shaper statistics

Inspect:

`diagnose sys session list`
Then correlate:

```
User
 ↓
Authentication
 ↓
Group
 ↓
Policy
 ↓
Session
 ↓
Shaper
```
| Symptom | Primary Checks | 
|---|---|
| Authentication fails | Server IP, secret, port, authentication method | 
| RADIUS timeout | Routing, firewall, UDP/1812 | 
| Accounting missing | UDP/1813, accounting configuration | 
| User authenticates but wrong policy matches | Group mapping, VSA, Filter-Id | 
| RSSO user not detected | Accounting, RSSO configuration, user/IP association | 
| Dynamic shaping fails | RADIUS attributes, accounting, policy, shaper | 
| Wrong group | NPS policy, VSA, Filter-Id | 
| Wireless session remains stale | Accounting, termination behavior | 
| CoA fails | CoA configuration, reachability, server support | 
| Username mismatch | Case sensitivity, username format | 
| MSCHAPv2 fails | NPS policy, AD integration, authentication method | 
| Policy does not match after successful login | Group mapping and returned attributes | 

```
                    Active Directory
                           │
                           ▼
                    ┌─────────────┐
                    │     NPS     │
                    │   RADIUS    │
                    └──────┬──────┘
                           │
              ┌────────────┴────────────┐
              │                         │
          UDP 1812                  UDP 1813
       Authentication             Accounting
              │                         │
              └────────────┬────────────┘
                           ▼
                    ┌─────────────┐
                    │  FortiGate  │
                    └──────┬──────┘
                           │
          ┌────────────────┼─────────────────┐
          │                │                 │
          ▼                ▼                 ▼
       Groups             RSSO        Dynamic Shaping
          │                │                 │
          └────────────────┼─────────────────┘
                           ▼
                    Firewall Policy
                           │
                           ▼
                          User
```
- AD user exists
- AD group membership is correct
- NPS receives request
- NPS policy matches
- Access-Accept is returned
- Required AVP/VSA is returned
- FortiGate recognizes the user
- Correct FortiGate group is selected
- Correct firewall policy matches
- Accounting starts
- RSSO learns the session when required
- Dynamic shaping applies when configured

```
Authentication
    ↓
Who are you?
Authorization
    ↓
What can you access?
```
Therefore:

```
RADIUS Authentication = SUCCESS
```
does **not** guarantee:

```
Firewall Policy Match = SUCCESS
```
```
1812 → Authentication
1813 → Accounting
```
If authentication works but RSSO/accounting-dependent behavior fails:

```
Check UDP/1813
```
```
AVP
 ↓
Standard RADIUS Attribute
VSA
 ↓
Vendor-Specific Attribute
```
Fortinet-specific information commonly uses the vendor-specific mechanism.

Always inspect:

```
AD
 ↓
NPS
 ↓
RADIUS Attribute
 ↓
FortiGate Group
 ↓
Firewall Policy
```
RSSO is not simply:

```
Username + Password
```
Think:

```
RADIUS Authentication
       +
Accounting / Session Information
       ↓
FortiGate User ↔ IP Awareness
```
If authentication works but shaping does not:

```
RADIUS Attributes
        ↓
Accounting
        ↓
User / Session
        ↓
Firewall Policy
        ↓
Dynamic Shaper
```
Trace the complete chain.

- RADIUS server IP is correct
- Shared secret is strong
- Shared secret matches on both sides
- UDP/1812 is restricted to trusted clients
- UDP/1813 is restricted to trusted clients
- NAS IP is correct
- NPS policies are least-privilege
- Authentication method is appropriate
- AD integration is functioning
- Authentication logging is enabled

- RADIUS server object configured
- Correct RADIUS group configured
- Firewall policy references correct group
- Required VSA/AVP mappings validated
- Accounting configured where required
- RSSO configured where required
- CoA configured where required
- Dynamic shaping configured where required
- Authentication troubleshooting logs available

- Never publish RADIUS shared secrets
- Never publish production passwords
- Never publish OTP seeds
- Never publish private keys
- Restrict RADIUS clients to trusted NAS devices
- Monitor authentication failures
- Monitor abnormal accounting traffic
- Review NPS policies regularly
- Use secure management access
- Validate supported secure RADIUS options for the target environment

```
config user radius
    edit "rad-1"
        set server "192.168.20.200"
        set secret <RADIUS_SECRET>
    next
end
```
```
config user radius
    edit "rad-1"
        set password-encoding auto
        set username-case-sensitive enable
    next
end
```
```
config user radius
    edit "rad-1"
        config accounting-server
            edit 1
                set status enable
                set server 192.168.20.200
                set port 1813
                set secret <RADIUS_SECRET>
            next
        end
    next
end
```
```
config user radius
    edit "rad-1"
        set radius-coa enable
        set radius-all-server enable
    next
end
```
⚠️ Verify exact syntax against the target FortiOS release.

`diagnose test authserver radius <server> <username> <password>``diagnose firewall auth list``diagnose sys session list``diagnose firewall shaper dynamic-shaper stats``diagnose firewall shaper dynamic-shaper list``diagnose firewall shaper dynamic-shaper list ip 192.168.20.20``diagnose user-device-store user-count list 1`
Example:

```
diagnose user-device-store user-count query \
"CN=callcenter,OU=information-tech,OU=publish-users,DC=test,DC=com"
```
`diagnose wireless-controller wlac-d sta online````
                         RADIUS
                           │
             ┌─────────────┼─────────────┐
             │             │             │
            1812          1813          VSA
             │             │             │
      Authentication   Accounting    Vendor Data
             │             │             │
             └─────────────┼─────────────┘
                           │
                           ▼
                       FortiGate
                           │
              ┌────────────┼────────────┐
              │            │            │
           Groups         RSSO         CoA
              │            │            │
              │       User ↔ IP     Authorization
              │
              ▼
       Firewall Policy
              │
              ▼
           Session
              │
              ▼
      Dynamic Shaping
```
```
                    RADIUS
                       │
       ┌───────────────┼────────────────┐
       │               │                │
    1812/AAA         1813             VSA
       │               │                │
 Authentication    Accounting      Vendor Data
       │               │                │
       └───────────────┼────────────────┘
                       │
                       ▼
                    NPS/AD
                       │
                       ▼
                 Group Mapping
                       │
                       ▼
                  FortiGate
                       │
          ┌────────────┼────────────┐
          │            │            │
        Policy        RSSO         CoA
          │            │            │
          ▼            ▼            ▼
       Access       User/IP      Dynamic Auth
          │
          ▼
       Session
          │
          ▼
   Dynamic Shaping
```
- RADIUS server reachable
- UDP/1812 reachable
- Shared secret correct
- NAS identity correct
- Authentication method correct
- NPS policy matches
- AD authentication succeeds

- Username format verified
- Case sensitivity understood
- Required AVP/VSA returned
- Group mapping verified
- FortiGate group matches returned identity

- UDP/1813 reachable
- Accounting enabled
- Start packet received
- Interim update received when required
- Stop packet received

- RSSO configured
- Accounting working
- User/IP association learned
- RSSO group configured
- Policy references RSSO identity

- CoA supported and configured
- Required attributes returned
- Session can receive updates
- Authorization change is verified

- Dynamic shaping enabled
- Correct user/session identified
- Required RADIUS attributes present
- Correct firewall policy matched
- Shaper statistics verified

**FortiGate RADIUS is much more than username/password authentication.**


The enterprise identity chain is:

```
Active Directory
      ↓
NPS
      ↓
RADIUS Authentication
      ↓
AVP / VSA
      ↓
FortiGate Group
      ↓
Firewall Policy
      ↓
Session
      ↓
Accounting / RSSO
      ↓
CoA / Dynamic Authorization
      ↓
Dynamic Shaping
```
```
1. 1812 = Authentication
2. 1813 = Accounting
3. AVP = Standard RADIUS Attribute
4. VSA = Vendor-Specific Attribute
5. RSSO = RADIUS-based user/session awareness
6. CoA = Change of Authorization
7. Authentication Success ≠ Policy Match Success
```
```
If RADIUS authentication succeeds
but the user receives the wrong access:
DO NOT STOP AT RADIUS.
Trace:
AD
 ↓
NPS Policy
 ↓
RADIUS Response
 ↓
AVP / VSA
 ↓
FortiGate Group
 ↓
Firewall Policy
 ↓
Session
 ↓
Accounting / RSSO
 ↓
Shaper / Authorization
```
- 
YouTube — SheynShield 
  - Fortinet NSE content
  - FortiGate troubleshooting
  - Network Security Engineering

**SheynShield | Engineering Secure Networks**

`FortiGate` · `RADIUS` · `NPS` · `Active Directory` · `RSSO` · `VSA` · `AVP` · `CoA` · `Dynamic Shaping` · `Fortinet` · `Network Security` · `NSE4` · `NSE7`
