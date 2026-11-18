FortiOS Focus: DLP, Data Types, Dictionaries, Sensors, DLP Profiles, Fingerprinting, Proxy Inspection Audience: FortiGate / NSE4–NSE7 / Network & Security Engineers Primary Goal: Detect, log, block, quarantine, or restrict sensitive data leaving or entering the network Inspection Requirement: Proxy-based inspection for full DLP functionality Common Use Cases: Data leakage prevention, sensitive-file control, compliance, file inspection
| Component | Purpose | 
|---|---|
| DLP | Detects and controls sensitive information in network traffic | 
| Data Type | Defines what sensitive content should be detected | 
| Keyword | Matches specific words or phrases | 
| Regex | Matches data using regular expressions | 
| Hex | Matches binary/hexadecimal patterns | 
| Credit Card | Detects credit-card-like data | 
| SSN | Detects Social Security Number patterns | 
| Dictionary | Groups keywords/patterns into a logical detection set | 
| Sensor | Defines how DLP dictionaries/data types are evaluated | 
| DLP Profile | Defines protocol, file, action, and detection behavior | 
| Fingerprinting | Detects known files by comparing file fingerprints | 
| Watermarking | Detects data/files using embedded watermark information | 
| Proxy Inspection | Required for many advanced DLP inspection capabilities | 
| Deep Inspection | Allows inspection of encrypted HTTPS traffic where applicable | 
DLP — Data Leak Prevention is used to detect and control sensitive information moving through network traffic.
Typical use case:
Client
   │
   │ Upload / Download
   ▼
FortiGate
   │
   ▼
DLP Inspection
   │
   ├── Detect sensitive data
   ├── Log
   ├── Allow
   ├── Block
   └── Quarantine
   │
   ▼
Internet / External Service
Credentials
Personal information
Credit-card numbers
Social Security Numbers
Confidential documents
Source code
Internal keywords
Sensitive files
A simplified deployment:
                    INTERNET
                       │
                       ▼
                ┌──────────────┐
                │  FortiGate   │
                │              │
                │ Proxy        │
                │ Inspection   │
                │      │       │
                │      ▼       │
                │     DLP      │
                │      │       │
                │      ▼       │
                │ IPS / Other  │
                │ Profiles     │
                └──────┬───────┘
                       │
                       ▼
                     LAN
                    Clients
For HTTPS traffic:
HTTPS
  │
  ▼
SSL Deep Inspection
  │
  ▼
Decrypted Content
  │
  ▼
DLP Inspection
  │
  ▼
Other Security Profiles
  │
  ▼
Destination
Key idea: If FortiGate cannot inspect the actual content, DLP cannot reliably detect sensitive information inside that content.
One of the most important NSE concepts:
DLP
 │
 └── Proxy Inspection
For a firewall policy using DLP:
Firewall Policy
      │
      ├── Proxy Mode
      ├── Deep Inspection
      └── DLP Profile
LAN
 │
 ▼
Firewall Policy
 │
 ├── Proxy Inspection
 ├── SSL Deep Inspection
 ├── DLP Profile
 └── IPS / Other Security Profiles
 │
 ▼
Internet
Exam note: When using DLP features that require proxy processing, a flow-based policy is not sufficient.
If DLP options are hidden in the GUI:
System
  └── Feature Visibility
        └── DLP
Enable the DLP feature.
Depending on the FortiOS release, the GUI terminology may appear as:
Data Leak Prevention
or
DLP
Think of DLP as a hierarchy:
Data Types
    │
    ▼
Dictionaries
    │
    ▼
Sensors
    │
    ▼
DLP Profile
    │
    ▼
Firewall Policy
Or:
"What?"
   ↓
Data Type
"What group?"
   ↓
Dictionary
"How should it match?"
   ↓
Sensor
"What should FortiGate do?"
   ↓
DLP Profile
"Where should it apply?"
   ↓
Firewall Policy
DLP Data Types define the content that FortiGate should detect.
Common types:
Keyword
Regex
Hex
Credit Card
Social Security Number
Custom
Matches a specific word or phrase.
Example:
CONFIDENTIAL
INTERNAL
SECRET
Uses a regular expression to detect a pattern.
Example:
\b\d{3}-\d{2}-\d{4}\b
This can be used as part of a pattern for identifying SSN-like data.
Allows detection based on hexadecimal data patterns.
Useful when the sensitive content is represented in binary/hex form.
FortiGate can use built-in or customized patterns for credit-card-like information.
Can be detected using built-in or custom matching/verification patterns.
A DLP Data Type can use built-in patterns.
Example:
config dlp data-type
    edit "keyword"
        set pattern built-in
    next
    edit "regex"
        set pattern built-in
    next
    edit "hex"
        set pattern built-in
    next
end
Built-in patterns are supplied by FortiOS.
Custom patterns allow the administrator to define organization-specific detection logic.
Example conceptual configuration:
config dlp data-type
    edit "credit-card"
        set pattern "\\b([2-6]{1}\\d{3})[- ]?(\\d{4})[- ]?(\\d{2})[- ]?(\\d{2})[- ]?(\\d{2,4})\\b"
        set verify built-in
        set look-back 20
        set transform "\\b\\1[- ]?\\2[- ]?\\3[- ]?\\4[- ]?\\5\\b"
    next
end
| Parameter | Meaning | 
|---|---|
| pattern | Pattern used for detection | 
| verify | Additional validation/verification logic | 
| look-back | Context window around the detected value | 
| transform | Defines how matched capture groups are represented/transformed | 
Exact syntax and supported options should always be checked against the FortiOS version being used.
Example:
config dlp data-type
    edit "ssn-us"
        set pattern "\\b(\\d{3})-(\\d{2})-(\\d{4})\\b"
        set verify "(?<!-)\\b(?!666|000|9\\d{2})\\d{3}-(?!00)\\d{2}-(?!0{4})\\d{4}\\b(?!-)"
        set look-back 12
        set transform "\\b\\1-\\2-\\3\\b"
    next
end
Conceptually:
Raw Content
    │
    ▼
Pattern Match
    │
    ▼
Verification
    │
    ▼
Context Check
    │
    ▼
DLP Detection
A Dictionary groups patterns that belong to a logical category.
Example:
DLP Dictionary
      │
      ├── Keyword: CONFIDENTIAL
      ├── Keyword: INTERNAL
      ├── Keyword: SECRET
      └── Custom Pattern
GUI path:
Security Profiles
  └── Data Leak Prevention
        └── Dictionary
A dictionary can evaluate entries using logical relationships.
Keyword A
   OR
Keyword B
   OR
Keyword C
If one entry matches:
MATCH
Example:
Dictionary
 ├── PASSWORD
 ├── CONFIDENTIAL
 └── SECRET
Match Type = ANY
One matching entry can trigger the configured behavior.
Keyword A
   AND
Keyword B
   AND
Keyword C
All required entries must match.
Match Type = ALL
Dictionary: dlp-dic-test
Logical Relationship:
    ANY
Entries:
    ├── Type: Keyword
    │   Pattern: dlptest
    │
    └── Type: Keyword
        Pattern: confidential
Conceptually:
dlptest
   OR
confidential
   ↓
Dictionary Match
A DLP Sensor defines how dictionaries and detection entries are evaluated.
Architecture:
Data Type
   │
   ▼
Dictionary
   │
   ▼
Sensor
   │
   ▼
DLP Profile
Example:
config dlp sensor
    edit "sen-test"
        set match-type match-any
        config entries
            edit 1
                set dictionary "dlp-dic-test"
                set count 1
                set status enable
            next
        end
    next
end
Common logical behavior:
match-any
means:
Entry 1
   OR
Entry 2
   OR
Entry 3
Example:
Sensor
 │
 ├── Dictionary A
 ├── Dictionary B
 └── Dictionary C
match-any
A matching entry can trigger the sensor.
Example:
set count 1
Conceptually:
Dictionary Match Count >= 1
          ↓
Sensor Trigger
If the configured count is higher:
count 5
the configured detection must meet that threshold before the sensor triggers.
The DLP Profile determines what FortiGate should do after detecting sensitive information.
GUI:
Security Profiles
  └── Data Leak Prevention
        └── Profile
Typical actions include:
Allow
Log Only
Block
Quarantine
Conceptual configuration:
config dlp profile
    edit "dlp-prof-test"
        set feature-set proxy
        config rule
            edit 1
                set proto http-get
                set filter-by none
                set file-type 1
                set file-size 500
                set action block
            next
        end
    next
end
set feature-set proxy
is important when the DLP feature requires proxy-based processing.
A DLP rule can control:
Protocol
Filter
File Type
File Size
Action
Example:
DLP Rule
 │
 ├── Protocol: HTTP
 ├── Filter: File Type
 ├── File Type: Built-in
 ├── File Size: 500 KB
 └── Action: Block
DLP can be restricted to specific file types.
Example concept:
File Type
   │
   ├── Documents
   ├── Archives
   ├── Images
   ├── Executables
   └── Other types
The exact numeric file-type IDs depend on the FortiOS implementation.
NSE note: Do not memorize numeric IDs without verifying them against the target FortiOS release.
Example:
set file-size 500
Conceptually:
Maximum / configured file size
        ↓
500 KB
This allows administrators to limit which files are inspected by a particular DLP rule.
Common DLP actions:
| Action | Behavior | 
|---|---|
| Allow | Permit the activity | 
| Log Only | Allow while recording the violation | 
| Block | Block the matching activity | 
| Quarantine | Restrict the violating source/activity according to the profile behavior | 
LOG ONLY
   ↓
Detect + Log
   ↓
Traffic continues
BLOCK
   ↓
Matching activity is denied
QUARANTINE
   ↓
Stronger restriction behavior
   ↓
Used when repeated/serious violations need containment
Exact quarantine behavior can vary by FortiOS version and DLP configuration.
A useful DLP concept:
Filters are ordered, but the possible actions do not have precedence over each other simply because they appear in a particular order.
Think of:
Filter 1
Filter 2
Filter 3
as ordered evaluation logic, while:
Allow
Block
Quarantine
Log
are configured actions rather than an inherent priority hierarchy.
Typical deployment:
LAN
 │
 ▼
Firewall Policy
 │
 ├── Proxy Inspection
 ├── Deep Inspection
 ├── DLP Profile
 └── IPS Profile
 │
 ▼
Internet
Source      : LAN
Destination : Internet
Service     : Required services
Inspection  : Proxy
SSL         : Deep Inspection
DLP         : dlp-prof-test
IPS         : Required IPS Sensor
NAT         : Enable
For encrypted web traffic:
Client
  │
  │ HTTPS
  ▼
FortiGate
  │
  ├── SSL Deep Inspection
  │
  ▼
Decrypted Content
  │
  ├── DLP
  ├── IPS
  ├── Application Control
  └── Web Filtering
  │
  ▼
Internet
Without decryption:
HTTPS
  │
  ▼
Encrypted Payload
  │
  X
DLP cannot inspect hidden content
Important: SSL inspection and DLP should be designed together when the goal is preventing leakage through HTTPS.
DLP introduces additional processing overhead.
Traffic
   ↓
Proxy
   ↓
Content Processing
   ↓
DLP
   ↓
Other Security Profiles
   ↓
Forwarding
Compared with simple firewall forwarding:
More inspection
      ↓
More CPU / Memory
      ↓
Potentially lower throughput
- Size FortiGate appropriately
- Test expected traffic volume
- Consider file size limits
- Avoid inspecting unnecessary content
- Use targeted DLP rules
- Monitor CPU and memory
- Test cloud applications separately
Cloud services can create special DLP challenges.
Examples:
Google Drive
SharePoint
Cloud Storage
Web-based SaaS
A browser upload does not always look like a simple:
HTTP POST
   ↓
file.pdf
Cloud applications may use:
Proprietary APIs
Custom encoding
Multiple HTTP requests
Metadata APIs
Chunked uploads
Dynamic endpoints
A practical issue can occur where:
File is blocked ✓
       │
       ▼
DLP detection works ✓
       │
       ▼
Filename in log
may be inaccurate / incomplete
This has been observed especially with some cloud-based services.
The application may transfer the file using:
Custom API
     +
Encoded content
     +
Separate metadata
instead of a traditional HTTP file-upload mechanism.
When blocking files, prefer file type when the requirement is actually based on file type.
Block:
secret-file-final-v2.pdf
using only a filename pattern.
Block:
*.pdf
using the appropriate DLP file-type classification.
Reason: Cloud applications may change how filenames and metadata are exchanged through their APIs.
FortiGate DLP can use several detection mechanisms:
                 DLP Detection
                       │
       ┌───────────────┼────────────────┐
       ▼               ▼                ▼
   Data Types      Dictionaries      Sensors
       │               │                │
       ├── Keyword     ├── Keyword      └── Logic
       ├── Regex       └── Patterns
       ├── Hex
       ├── CC
       └── SSN
                       │
                       ▼
                  Fingerprinting
                       │
                       ▼
                   Watermarking
DLP Fingerprinting can detect known sensitive documents by comparing fingerprints generated from files.
Conceptually:
Sensitive File
      │
      ▼
FortiGate
      │
      ▼
Generate Fingerprint
      │
      ▼
Store Fingerprint
      │
      ▼
Future Network Traffic
      │
      ▼
Generate Fingerprint
      │
      ▼
Compare
      │
   ┌──┴──┐
   ▼     ▼
 Match  No Match
   │       │
   ▼       ▼
 Action   Continue
| Method | Detects | 
|---|---|
| Keyword | Specific words/phrases | 
| Regex | Structured patterns | 
| Dictionary | Groups of matching terms | 
| Sensor | Matching logic | 
| Fingerprint | Known document/file content | 
| Watermark | Files/data containing recognized watermark information | 
Keyword
   ↓
"What does the content say?"
Fingerprint
   ↓
"Is this the known document?"
A document fingerprint source can be configured from a file repository.
Example source:
SMB Server
    │
    ▼
FortiGate
    │
    ▼
Document Source
    │
    ▼
Fingerprint Database
Important: The document fingerprint feature requires a FortiGate with internal storage.
Example configuration:
config dlp fp-doc-source
    edit "dlp-doc-test"
        set server-type smb
        set server 192.168.20.200
        set period daily
        set vdom root
        set scan-subdirectories enable
        set remove-deleted disable
        set keep-modified enable
        set username "<SERVICE_ACCOUNT>"
        set password "<SECRET>"
        set file-path "c:/w.pdf"
        set file-pattern "w.pdf"
        set sensitivity critical
        set tod-hour 00
        set tod-min 00
    next
end
Never publish real credentials:
❌ set username admin
❌ set password RealPassword123
Use:
<USER>
<SECRET>
in documentation and GitHub repositories.
Conceptually:
Sensitivity
    │
    ├── Warning
    ├── Low
    ├── Private
    ├── Medium
    └── Critical
The exact available values and mapping depend on the FortiOS implementation.
Example:
set sensitivity critical
A fingerprint rule can define a match percentage.
Example:
config dlp profile
    edit "dlp-prof-test"
        config filter
            edit 1
                set proto http-get
                set filter-by fingerprint
                set sensitivity critical
                set match-percentage 40
                set action block
            next
        end
    next
end
Conceptually:
Captured File
      │
      ▼
Fingerprint Comparison
      │
      ▼
Match Percentage
      │
      ├── < 40%
      │      ↓
      │   No Match
      │
      └── ≥ 40%
             ↓
          Match
             ↓
           Action
Common concepts:
Block
Ban
Quarantine
Restricts the matching DLP activity.
Violation
   ↓
Block
Can be used for stronger source restriction, particularly where repeated or abusive activity needs to be contained.
Repeated violation
       ↓
BAN
       ↓
Source restriction
Provides stronger restriction behavior for the violating source/activity according to the configured DLP policy.
Violation
   ↓
Quarantine
   ↓
Restricted activity
Exact timeout and scope behavior should be verified against the FortiOS release and DLP profile configuration.
             DOCUMENT REPOSITORY
                     │
                     ▼
              FortiGate scans
                     │
                     ▼
              Fingerprint DB
                     │
                     ▼
              Network Traffic
                     │
                     ▼
             DLP Fingerprint
                     │
                     ▼
                 Compare
                     │
              ┌──────┴──────┐
              ▼             ▼
            Match         No Match
              │             │
              ▼             ▼
           Action         Allow
Diagnostic command:
diagnose test application dlpfingerprint
Possible menu options include:
1   Show fingerprint daemon menu
2   Dump the database
3   Dump all files
5   Dump all chunks
6   Refresh all document sources in all VDOMs
7   Show database file size and limit
9   Display statistics
10  Clear statistics
99  Restart the daemon
Fingerprint not matching
        │
        ▼
Check Document Source
        │
        ▼
Refresh Database
        │
        ▼
Check Fingerprint DB
        │
        ▼
Check Statistics
        │
        ▼
Check DLP Profile
        │
        ▼
Test Traffic
[ ] DLP feature enabled
[ ] Correct DLP profile selected
[ ] Firewall policy uses proxy mode
[ ] Deep inspection enabled for HTTPS
[ ] Correct DLP sensor configured
[ ] Correct dictionary configured
[ ] Correct data type configured
[ ] Sensor match type verified
[ ] Rule protocol matches traffic
[ ] File type matches traffic
[ ] File size is within configured limit
[ ] Action is correctly configured
[ ] Logs enabled
Check:
HTTPS
  ↓
SSL Deep Inspection
  ↓
Decrypted Content
  ↓
DLP
  ↓
DLP Match?
If the content remains encrypted:
DLP
  ↓
Cannot inspect actual payload
Investigate:
Cloud Service
      ↓
Custom API
      ↓
Encoding / Metadata
      ↓
DLP Detection
      ↓
File blocked ✓
      │
      └── Filename logging may be inaccurate
Do not assume the DLP engine failed simply because the filename in the log is incorrect.
Recommended enterprise architecture:
                       INTERNET
                           │
                           ▼
                  ┌────────────────┐
                  │   FortiGate    │
                  │                │
                  │ SSL Inspection │
                  │      ↓         │
                  │     DLP        │
                  │      ↓         │
                  │     IPS        │
                  │      ↓         │
                  │ Web/App Control│
                  └───────┬────────┘
                          │
                          ▼
                         LAN
A strong DLP deployment can combine:
                DLP Security
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
   Data Types    Fingerprint    Watermark
       │             │             │
       └─────────────┼─────────────┘
                     ▼
                  Sensor
                     │
                     ▼
                DLP Profile
                     │
                     ▼
              Firewall Policy
                     │
                     ▼
            SSL Deep Inspection
DLP does not have to operate alone.
Example:
Traffic
   │
   ▼
SSL Deep Inspection
   │
   ▼
DLP
   │
   ├── Sensitive Data?
   │
   ▼
IPS
   │
   ▼
Application Control
   │
   ▼
Web Filter
   │
   ▼
Forward
This creates a layered security architecture.
DLP inspection can consume additional resources because FortiGate may need to:
Receive content
      ↓
Buffer / process content
      ↓
Inspect content
      ↓
Compare patterns
      ↓
Run additional security profiles
      ↓
Forward traffic
- Limiting unnecessary protocols
- Limiting file sizes
- Selecting relevant file types
- Using targeted sensors
- Avoiding unnecessary inspection
- Monitoring CPU/memory
- Testing cloud applications
- Validating throughput under real workloads
| Topic | Remember | 
|---|---|
| DLP | Prevents sensitive-data leakage | 
| Data Type | Defines detectable content | 
| Keyword | Matches words/phrases | 
| Regex | Matches structured patterns | 
| Dictionary | Groups matching entries | 
| Sensor | Defines detection logic | 
| DLP Profile | Defines inspection/action behavior | 
| Proxy Mode | Important for DLP processing | 
| Deep Inspection | Required to inspect decrypted HTTPS content | 
| Fingerprinting | Detects known files/documents | 
| Watermarking | Detects recognized watermark information | 
| Block | Blocks matching DLP activity | 
| Log Only | Detects/logs without blocking | 
| Quarantine | Stronger restriction behavior | 
| Ban | Strong source restriction for abusive activity | 
| Cloud DLP | API/encoding can affect filename logging | 
| File Type | Prefer when the requirement is based on file type | 
| Internal Storage | Required for document fingerprinting | 
| Resource Usage | DLP adds processing overhead | 
The easiest way to remember FortiGate DLP:
                 FORTIGATE DLP
                       │
                       ▼
                  WHAT TO FIND?
                       │
                       ▼
                  DATA TYPE
                       │
                       ▼
                HOW TO GROUP IT?
                       │
                       ▼
                  DICTIONARY
                       │
                       ▼
                HOW TO EVALUATE?
                       │
                       ▼
                    SENSOR
                       │
                       ▼
              WHAT SHOULD HAPPEN?
                       │
                       ▼
                 DLP PROFILE
                       │
                       ▼
               WHERE TO APPLY?
                       │
                       ▼
              FIREWALL POLICY
                  CLIENT
                     │
                     │ HTTPS
                     ▼
               ┌─────────────┐
               │  FortiGate  │
               └──────┬──────┘
                      │
                      ▼
              SSL Deep Inspection
                      │
                      ▼
                Decrypted Data
                      │
                      ▼
                     DLP
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
       Keyword      Regex     Fingerprint
          │           │           │
          └───────────┼───────────┘
                      ▼
                   Sensor
                      │
                      ▼
                 DLP Profile
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
         Allow       Block    Quarantine
                      │
                      ▼
                    Log
Flow
  ↓
DLP
  X
Use the appropriate proxy-based configuration when required by the DLP feature.
HTTPS
  ↓
Encrypted
  ↓
DLP
  X
Instead:
HTTPS
  ↓
SSL Deep Inspection
  ↓
DLP
  ✓
ALL TRAFFIC
     ↓
FULL DLP
     ↓
High Resource Usage
Prefer targeted inspection.
Especially with cloud applications:
Cloud API
   ↓
Custom Metadata
   ↓
Filename may not be reliable
When the goal is blocking a file type, use the appropriate file-type filter where possible.
Never commit:
username
password
API key
secret
private key
to GitHub.
Use placeholders:
<USERNAME>
<PASSWORD>
<SECRET>
DLP Deployment
│
├── [ ] Enable DLP feature
├── [ ] Enable proxy inspection
├── [ ] Configure SSL Deep Inspection where required
├── [ ] Define Data Types
├── [ ] Create Dictionaries
├── [ ] Configure Sensors
├── [ ] Create DLP Profile
├── [ ] Select protocols
├── [ ] Select file types
├── [ ] Define file-size limits
├── [ ] Select actions
├── [ ] Attach DLP to firewall policy
├── [ ] Enable required logging
├── [ ] Test HTTP
├── [ ] Test HTTPS
├── [ ] Test file upload
├── [ ] Test file download
├── [ ] Test cloud storage
├── [ ] Monitor CPU/memory
└── [ ] Validate false positives
Use a controlled test domain:
dlptest.com
Test flow:
Client
  │
  ▼
HTTPS
  │
  ▼
FortiGate
  │
  ├── SSL Inspection
  │
  ├── DLP
  │
  └── IPS
  │
  ▼
Test Destination
Test cases:
1. Keyword detection
2. Regex detection
3. Dictionary match
4. Sensor threshold
5. File-type filtering
6. File-size filtering
7. Block action
8. Log-only action
9. Fingerprint matching
10. HTTPS upload
- SSL Deep Inspection → Required when DLP must inspect encrypted HTTPS content
- Proxy-Based Inspection → Core processing model for advanced DLP
- IPS → Additional inspection layer
- Application Control → Control applications used for data transfer
- Web Filter → Control destination websites
- File Filtering → Restrict file types
- DLP Fingerprinting → Detect known sensitive documents
SheynShield Engineering Note
Don't think of FortiGate DLP as simply "block a keyword."
The real architecture is:
Data Type → Dictionary → Sensor → DLP Profile → Firewall Policy
And for encrypted web traffic:
HTTPS → SSL Deep Inspection → DLP → Action
For known sensitive documents:
Document Source → Fingerprint Database → Fingerprint Match → DLP Action
Finally, when troubleshooting cloud-storage DLP, separate detection accuracy from metadata/logging accuracy. A file can be successfully detected and blocked even when a cloud application's API causes the filename recorded in the log to be incomplete or inaccurate.
- 
YouTube — SheynShield 
  - Fortinet NSE content
  - FortiGate troubleshooting
  - Network Security Engineering