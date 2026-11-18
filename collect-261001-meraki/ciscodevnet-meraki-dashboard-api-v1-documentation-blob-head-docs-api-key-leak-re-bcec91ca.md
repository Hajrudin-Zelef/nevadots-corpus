If a Meraki Dashboard API key has been exposed or compromised, immediate action is required to secure your organization. API keys grant access to your network configurations and data, so treating a leak with urgency is critical.
This guide provides step-by-step procedures for responding to an API key leak, investigating the incident, and preventing future exposures.
⚠️ Important Disclaimer
This guide is not legal advice and is not intended to override or replace your organization's internal security practices, policies, or procedures.
This guide represents general industry best practices and Meraki-specific recommendations for responding to API key leaks. However, you must first and foremost follow your own organization's security breach procedures, incident response plans, and legal requirements. Your organization may have specific policies, compliance obligations, or regulatory requirements that supplement or supersede the guidance provided here.
Before taking any action, consult with:
- Your organization's security team or Security Operations Center (SOC)
- Your legal or compliance department
- Your incident response team or designated security contacts
- Any applicable industry-specific regulatory requirements (HIPAA, PCI-DSS, GDPR, etc.)
This guide should be used as a reference and starting point only, not as a replacement for your organization's established procedures or as legal counsel.
First, determine who owns the compromised API key. Each API key is associated with a specific admin identity (email address).
If the key belongs to your own email address:
- Proceed directly to Step 2
If you recognize the key but do not own it:
- Record the email address and name of the admin who owns the key
- Contact the owner immediately if they are a current employee
- If the owner has left the organization or is unresponsive, proceed with the admin removal process (see Step 3)
If you cannot identify the key owner but believe the key has access to your dashboard organization:
- Contact your organization's security team
- Review the list of admins in Dashboard (Organization > Admins)
- Check API activity logs to correlate the key with recent API calls
For your own API key:
- Sign in to Meraki Dashboard: https://dashboard.meraki.com
- Navigate to Organization >API & Webhooks
- Select API keys and access from the top tabs
- Locate the compromised API key in the list
- Click Revoke next to the compromised key
- Confirm the revocation
⚠️ Critical: Once revoked, the API key will immediately stop working. Any applications or scripts using this key will fail. Have a replacement key ready before revoking if you need to maintain service continuity.
If the API key belongs to a different admin who has access to your organization:
- Sign in to Meraki Dashboard: https://dashboard.meraki.com
- Navigate to Organization >Admins
- Locate the admin account associated with the compromised key
- Review all organizations to which this admin has access
- Remove the admin from each organization:
  - Click on the admin's name
  - Select Delete or Remove from organization
  - Confirm the removal
- Document the admin's email address, name, and removal date for your records
Note: Removing an admin from an organization immediately revokes their access to that organization regardless of which API key(s) they have. If they have access to multiple organizations, remove them from each one.
Use Meraki's API analytics to check for unauthorized or unexpected API activity:
- 
Sign in to Meraki Dashboard: https://dashboard.meraki.com
- 
Navigate to Organization >API & Webhooks
- 
Select API usage orAPI logs from the tabs
- 
Review recent API calls for suspicious activity: 
  - Look for:
    - API calls from unfamiliar IP addresses
    - Calls during unusual hours (nights, weekends)
    - High-volume requests (potential data exfiltration)
    - Calls to sensitive endpoints (configuration changes, deletions)
    - Failed authentication attempts (401/403 errors)
  - Filter by:
    - The compromised admin's identity
    - Date range covering the exposure window
    - Specific API operations (GET, POST, PUT, DELETE)
- Look for:
- 
Document any suspicious activity: 
  - Timestamp of suspicious calls
  - Source IP addresses
  - API endpoints accessed
  - Operations performed (reads vs. writes)
- 
Review activity for other admins as well to ensure no lateral movement or additional compromises
If you need to restore API access after revoking a compromised key:
- Sign in to Meraki Dashboard using the admin account that will provide the new API key: https://dashboard.meraki.com
- Navigate to Organization >API & Webhooks
- Select API keys and access from the top tabs
- Click Generate new API key
- Copy the new API key immediately (it will only be displayed once)
- Store the new key securely in your environment variables or secrets manager
Update all applications, scripts, and services that use the API key:
- 
Identify all systems using the compromised key: 
  - Development, staging, and production environments
  - Automated scripts and cron jobs
  - CI/CD pipelines
  - Third-party integrations
  - Documentation and runbooks
- 
Deploy the new API key to each system
- 
Test each application to verify the new key works
- 
Monitor for errors or failed authentication attempts
After containing the immediate threat, investigate how the API key was leaked and what damage may have occurred.
Common sources of API key leaks include:
- 
Version Control Systems 
  - Check git commit history for hardcoded keys
  - Search GitHub, GitLab, Bitbucket for public repositories containing your key
  - Use tools like git-secrets or truffleHog to scan repositories
- 
Logs and Monitoring Systems 
  - Review application logs for accidentally logged keys
  - Check centralized logging systems (Splunk, ELK, CloudWatch)
  - Search CI/CD build logs
- 
Documentation and Wikis 
  - Search internal documentation for example code with real keys
  - Check shared note-taking apps (Confluence, Notion, OneNote)
  - Review training materials and runbooks
- 
Communication Channels 
  - Search Slack, Teams, or email for keys shared in messages
  - Check support tickets or help desk systems
  - Review screenshare recordings or recorded meetings
- 
Public Exposure 
  - Search paste sites (Pastebin, GitHub Gists)
  - Check Stack Overflow and developer forums
  - Review any public-facing applications or API proxies
This list is illustrative but not exhaustive. Consult your organization's incident response team for additional avenues of exposure.
If you have removed an admin's access or identified a leak from another team member's key:
- Contact the key owner to inform them of the incident
- Explain what happened and the actions you've taken
- Ask them to:
  - Confirm which systems were using the key
  - Help identify how the key was exposed
  - Review their security practices
- Provide guidance on preventing future leaks (see Prevention section below)
Determine what unauthorized actions, if any, were taken using the compromised key:
- 
Review API logs for the entire exposure window
- 
Identify unauthorized operations: 
  - Data access: Were any sensitive network configurations or device details accessed?
  - Configuration changes: Were any networks, devices, or settings modified?
  - Data exfiltration: Was there high-volume data retrieval?
  - Destructive actions: Were any resources deleted or disabled?
- 
Document findings: 
  - Timeline of unauthorized activity
  - Scope of data accessed or modified
  - Business impact assessment
  - Compliance implications (GDPR, HIPAA, PCI-DSS, etc.)
- 
Remediate unauthorized changes: 
  - Revert configuration changes if necessary
  - Review and validate network settings
  - Check for backdoors or persistence mechanisms
Depending on your organization's regulatory requirements, you may need to:
- 
Report the incident internally: 
  - Notify your security team
  - Inform management and stakeholders
  - Document the incident for security records
- 
Report the incident externally (if required): 
  - Notify affected customers if their data was accessed
  - File breach reports with regulatory bodies (GDPR requires notification within 72 hours)
  - Inform cyber insurance provider
- 
Preserve evidence: 
  - Save API logs showing unauthorized access
  - Document the timeline of the incident
  - Retain copies of communications related to the incident
Implement these practices to prevent future API key leaks:
Never hardcode API keys in source code. Always use one of these secure storage methods:
- 
Environment Variables (recommended for local development) 
  - Store keys in MERAKI_DASHBOARD_API_KEY environment variable
  - Add .env files to.gitignore
  - Use tools like python-dotenv ordirenv to manage environment variables
- Store keys in 
- 
Secrets Management Systems (recommended for production) 
  - HashiCorp Vault
  - AWS Secrets Manager
  - Azure Key Vault
  - Google Cloud Secret Manager
  - 1Password Secrets Automation
  - Doppler
- 
CI/CD Secret Stores 
  - GitHub Secrets
  - GitLab CI/CD Variables
  - Jenkins Credentials
  - CircleCI Environment Variables
- 
Add API keys to .gitignore :.env
.env.local
config/secrets.yml
**/api_keys.txt
- 
Use pre-commit hooks: 
  - Install git-secrets
  - Configure pre-commit with secret detection
- 
Scan repositories regularly: 
  - Use truffleHog to scan for secrets
  - Enable GitHub's secret scanning feature
  - Use GitGuardian or similar tools for continuous monitoring
- 
If you accidentally commit a key: 
  - Revoke the key immediately (don't wait to clean git history first)
  - Remove the key from git history using git filter-branch or BFG Repo-Cleaner
  - Force push the cleaned history (coordinate with team)
  - Remember: public commits may be cached by search engines or GitHub's API
Establish a routine API key rotation schedule. These are only suggestions; consult your organization's security team for any specific requirements that may override or replace these suggestions.
- 
Recommended rotation frequency: 
  - Every 30-90 days is a common baseline.
  - After any personnel changes, if the admin ever had access to the API key, and the API key didn't belong to their admin user, then rotate the key immediately. If the admin never had access to the key, or if the key belonged to that admin, then deleting the admin from an org prevents the key from working in that org.
- 
Use Meraki's two-key system for zero-downtime rotation: 
  - Week 1: Generate second API key
  - Week 2: Deploy new key to all development/staging systems
  - Week 3: Deploy new key to production systems
  - Week 4: Verify new key functionality and revoke old key
- 
Automate rotation where possible: 
  - Use secrets managers with automatic rotation capabilities
  - Script the key generation and deployment process
  - Set calendar reminders for manual rotation
- 
Principle of Least Privilege: 
  - Create admin accounts with only the permissions required
  - Use read-only admin roles when write access isn't needed
  - Consider using OAuth 2.0 instead of API keys for applications requiring specific, limited permissions
- 
Multi-Factor Authentication: 
  - Enable 2FA for all admin accounts that can generate API keys
  - Use authenticator apps instead of SMS whenever possible
- 
Admin Lifecycle Management: 
  - Document all API keys and their purposes
  - Revoke API keys immediately when admins leave the organization
  - Review admin access quarterly
  - Maintain an inventory of active API keys
Set up monitoring to detect potential API key compromises:
- 
Establish baselines: 
  - Normal API call volume per hour/day
  - Typical API operations performed
  - Expected source IP addresses
  - Standard usage patterns (time of day, day of week)
- 
Configure alerts for anomalies: 
  - API calls from new geographic locations
  - High-volume requests (potential data exfiltration)
  - Failed authentication attempts (401/403 errors)
  - Calls to sensitive endpoints (DELETE operations, admin changes)
  - API usage during off-hours
- 
Regular log reviews: 
  - Review API logs weekly or monthly
  - Look for unusual patterns or unexpected activity
  - Investigate any rate limit breaches
- 
Rate limiting awareness: 
  - Meraki enforces 5 calls per second per organization
  - Unusual rate limiting errors may indicate unauthorized usage
  - Monitor for 429 (Too Many Requests) responses
Consider whether API keys are the best authentication method for your use case:
| Feature | OAuth 2.0 Grants | API Keys | 
|---|---|---|
| Best for | Third-party applications, organization-wide automation | Personal scripts, admin-specific tasks | 
| Scope | App-scoped with granular permissions | Admin-scoped based on user role | 
| Permissions | Configurable per application | Inherits from admin's role | 
| Identity | Application identity | Admin identity | 
| Management | Organization level | Individual admin level | 
| Token lifetime | 60 minutes (auto-refresh) | Permanent until revoked | 
| Security | Better isolation, shorter-lived tokens | Simpler but higher risk if exposed | 
Recommendation: For third-party applications or organization-wide automation, prefer OAuth 2.0 grants over API keys. OAuth tokens are short-lived (60 minutes) and have configurable, granular permissions. See the OAuth documentation for more information.
- 
Security training: 
  - Train developers on API key security best practices
  - Include security in onboarding for new team members
  - Conduct regular security awareness sessions
- 
Incident response drills: 
  - Practice API key leak response procedures
  - Test your team's ability to detect and respond quickly
  - Update procedures based on lessons learned
- 
Documentation: 
  - Maintain runbooks for key rotation and incident response
  - Document where API keys are used in your infrastructure
  - Keep contact information current for security incidents
Use this checklist when responding to an API key leak:
- Identify the owner of the compromised API key
- Revoke the compromised API key via Dashboard
- Remove admin access if owner has left the organization
- Generate a replacement API key (if needed)
- Notify your security team
- Audit API activity logs for suspicious behavior
- Identify source of the leak (git history, logs, etc.)
- Determine exposure window (when was key created vs. exposed)
- Assess damage and unauthorized actions taken
- Contact the key owner to discuss the incident
- Document findings and timeline
- Update all applications and services with new API key
- Revert any unauthorized configuration changes
- Test all systems to verify new key works
- Monitor API logs for continued suspicious activity
- File compliance reports if required
- Implement preventive measures (pre-commit hooks, secrets scanning)
- Update security procedures and documentation
- Conduct team training on API key security
- Set up monitoring and alerting for future incidents
- Schedule regular API key rotation
- Review and update access controls
- Authorization Documentation - Learn about API keys vs OAuth 2.0
- OAuth Documentation - Implementing OAuth for better security
- Getting Started Guide - Best practices for new API users
- Rate Limiting - Understanding API rate limits
- Meraki Developer Portal - Official Meraki API documentation
- Meraki Community - Get help from the Meraki community
If you need assistance responding to an API key leak:
- 
For urgent security incidents: 
  - Contact your organization's security team immediately and follow their instructions.
  - If you're a Meraki customer with support, open an urgent support case and provide information about which steps you've already taken.
- 
For general questions: 
  - Visit the Meraki Community
  - Review the official API documentation
  - Consult your organization's security policies and procedures
Remember: The most effective way to handle an API key leak is to prevent it in the first place. Always treat API keys like passwords, use secure storage methods, and implement regular rotation practices.