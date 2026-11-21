---
id: collect-261121-veille-ia/microsoft-actu/wiz-ties-microsoft-agent-framework-package-compromise-to-release-pipeline-certificate-siliconangle
title: "Wiz ties Microsoft Agent Framework package compromise to release pipeline certificate - SiliconANGLE"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-19"]
keywords: ["framework", "package", "microsoft", "agent", "compromise", "release", "pipeline", "certificate"]
source: https://siliconangle.com/2026/11/19/wiz-ties-microsoft-agent-framework-package-compromise-to-release-pipeline-certificate/
source_anchor: ""
source_lines: [1, 43]
section: "Microsoft_Actu"
---
### Wiz ties Microsoft Agent Framework package compromise to release pipeline certificate

Microsoft Corp.’s official Python package for its Agent Framework artificial intelligence agent toolkit was compromised earlier this week, as an attacker published two malicious versions to the Python Package Index that were designed to steal cloud and developer credentials.

The malicious versions, 1.26.1 and 1.26.2, were published on Tuesday through Microsoft’s own publisher account, giving them the appearance of a routine update to a package that sees close to 200,000 downloads a month. Both versions were pulled from PyPI within about an hour, but anyone who installed them in that window is at risk.

Researchers at cloud security company Wiz Inc. have since found that the attacker appears to have gained publishing access using a client certificate and private key for Microsoft’s Engineering System Release Pipeline, an internal service the company uses to sign and publish packages. Address filtering on the service blocked the attacker’s first attempt from Amazon Web Services, according to Wiz, though the attacker was able to publish on a second try from Microsoft-owned address space.

How the attacker obtained the credentials is not known. Microsoft had not responded to a request for comment from BleepingComputer as of Wednesday.

The researchers also observed part of the operation, as the attacker’s tooling was reporting its progress to the same public webhook it was using to collect stolen data. The tooling identified itself as Cairn, an open-source penetration-testing framework that uses AI.

The malware itself is delivered through a .pth file added to both releases, a type of file Python runs every time it starts, meaning the credential stealer runs even if the framework itself is never imported. The two versions also kept the genuine agent-framework-core 1.26.0 as a dependency, so the framework continued to work normally while the malware ran in the background.

Once running, the malware collects secrets from environment variables and from other running processes, along with credential files for services such as Azure, npm, Git, Docker and Hugging Face. It also requests Azure managed identity tokens for 12 resources, according to application security company Endor Labs Inc., with the second version adding requests for GitHub Actions tokens. The stolen data is then compressed, encoded and sent to a webhook.site address controlled by the attacker.

Researchers at malware tracking site OpenSourceMalware described the malware as a “focused credential and token stealer, not a remote access trojan,” noting that it does not install any persistence on infected systems.

Wiz’s analysis suggests the number of victims may be small, though it covers only the 50 requests the attacker’s free webhook.site account retained. Of those, 17 represented only three unique successful thefts of victim data, with the rest coming from the attacker’s own machines and from sandbox and security vendor scanning.

The incident is not the first this year involving an official Microsoft Python package. In May, Microsoft’s durabletask package was hijacked after its PyPI token was stolen in an attack that has been linked to TeamPCP, the group also behind the compromise of LiteLLM. Wiz says it has not linked the two incidents.

Anyone who installed either of the malicious versions is being urged to treat it as a credential compromise, according to researchers, who recommend rebuilding from version 1.26.0 and rotating any secrets the affected systems could access.

“A list of dependencies in production might show no trace of the framework, while a build runner could have installed it and been shut down before the alert arrived,” James Matchett, head of security at software supply chain security company Cloudsmith Ltd., told SiliconANGLE.

“If that runner had access to a deployment token, the team needs to know which version it installed and what the token could access,” he added.

##### Image: SiliconANGLE/GPT Image 2.6

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
