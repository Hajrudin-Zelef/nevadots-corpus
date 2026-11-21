---
id: collect-261121-veille-ia/google-ecosystem/google-builds-giant-fake-internet-called-mirror-dimension-to-safely-test-ai-agents-lapaas-voice
title: "Google Builds Giant Fake Internet Called ‘Mirror Dimension’ to Safely Test AI Agents - Lapaas Voice"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["websites", "agents", "google", "internet", "mirror", "dimension", "security", "environment"]
source: https://lapaasvoice.com/google-builds-giant-fake-internet-called-mirror-dimension-to-safely-test-ai-agents/
source_anchor: ""
source_lines: [1, 322]
section: "Google_Ecosystem"
---
Google has built a massive simulated version of the internet, called the “Mirror Dimension”, to test and strengthen the security of its artificial intelligence agents. Developed by Google’s Chrome security team, the environment contains thousands of realistic websites, including online stores, social media platforms, banking dashboards and other interactive services. It allows AI agents powered by Google’s Gemini models to browse websites and complete complex tasks without interacting with real users, accounts or financial systems.

Google revealed the project in a November 19, 2026 blog post, explaining that testing autonomous browsing agents on the live internet creates security and reliability risks. At the same time, basic simulations may not accurately reproduce the complexity of real websites. The Mirror Dimension addresses this problem by creating a controlled internet-like environment in which agents can perform realistic activities while researchers deliberately introduce security threats, including hidden instructions designed to manipulate their behaviour.

## Key Takeaways

- Google has developed a synthetic internet called **Mirror Dimension** to test AI browsing agents safely.
- The environment contains thousands of simulated websites covering shopping, banking, social media and other online activities.
- Agents interact with these websites through a real Chrome browser inside an isolated sandbox.
- Google uses the environment to test indirect prompt injection attacks and other security weaknesses.
- AI development agents help build and maintain realistic websites automatically.
- Google’s testing pipeline can generate more than 1,000 candidate security tests per security engineer every day.
- The system addresses a problem known as evaluation awareness, where AI models behave differently because they recognise that they are being tested.
- Google says the agents tested so far are benign and do not have cyber capabilities.

## What Is Google’s Mirror Dimension?

The Mirror Dimension is a controlled digital environment designed to reproduce the experience of browsing the public internet without allowing AI agents to access real websites.

It is not simply a collection of static demonstration pages.

Google has created an interconnected ecosystem of realistic websites and web applications that behave like their real-world equivalents.

The simulated environment includes:

- E-commerce websites with product catalogues and shopping carts.
- Banking dashboards with interactive account interfaces.
- Social media platforms with populated feeds.
- Local service marketplaces.
- Recipe websites and other everyday online services.
- Multi-page websites with forms, navigation and interactive features.

AI agents can browse these sites, click buttons, enter information, navigate between pages and complete multi-step tasks.

For example, an agent could be asked to find a product, apply a discount code and proceed through a simulated checkout process.

Another test might involve navigating a banking dashboard or researching information across multiple websites.

The important difference is that these activities take place entirely inside Google’s controlled environment.

No real customer accounts, purchases or financial transactions are involved.

## Why Google Needed to Build a Fake Internet

Google’s Chrome team identified a major problem with testing AI agents.

These systems are designed to navigate websites and perform tasks that require multiple actions. Testing them properly requires thousands of interactions across different websites.

However, using the real internet creates two significant problems.

### 1. Testing on real websites can create security risks

An AI agent may accidentally perform an action that has consequences outside the testing environment.

For example, an agent could:

- Submit a real online form.
- Send an unintended message.
- Modify account information.
- Place an actual order.
- Interact with a website containing malicious instructions.

These risks become more serious when agents are given permission to interact with applications and external services.

A controlled testing environment allows engineers to evaluate such behaviour without exposing real users or businesses to unintended actions.

### 2. Simple simulations are not realistic enough

Developers can create basic test websites, but these environments often lack the complexity of the real internet.

A real shopping website may include product filters, account login screens, discount codes, checkout processes and multiple navigation paths.

A basic demonstration page cannot reproduce all these interactions.

Google also identified a problem known as evaluation awareness.

This occurs when an AI model recognises that it is being tested and changes its behaviour accordingly.

If an agent realises that it is operating on an obviously artificial website, it may behave differently from how it would behave during a real-world task.

The Mirror Dimension attempts to reduce this problem by presenting agents with realistic websites and familiar URLs.

## How Google’s Mirror Dimension Works

Google has combined three main technologies to create the synthetic internet:

1. An isolated browser sandbox.
2. A local network interception proxy.
3. Realistic synthetic websites.

The combination allows an AI agent to navigate familiar internet addresses while receiving content from Google’s simulated environment.

<AsyncImage query=”Google Chrome Mirror Dimension synthetic internet sandbox architecture diagram proxy isolated browser” aspectRatio=”16:9″ width=”100%” maxHeight={260}/>

### Step 1: AI agents use a real Chrome browser

The system runs a headless Chrome browser inside an isolated gVisor sandbox.

Headless Chrome allows websites to load and function without displaying a normal browser window to a human user.

The agent can still interact with page elements, navigate between websites and perform actions.

Google uses this approach because testing agents through the actual browser provides a more realistic evaluation of how they interact with websites.

### Step 2: Internet traffic is redirected to simulated websites

One of the more interesting engineering challenges involves making the agent believe it is visiting ordinary websites.

For example, an agent may attempt to navigate to YouTube’s real web address.

Instead of allowing the browser to connect to the public internet, the sandbox captures the network request and sends it to a local proxy.

The proxy then serves a simulated version of the requested website.

From the agent’s perspective, the URL looks normal, but the content comes from Google’s synthetic environment.

This helps reduce the possibility that the model recognises it is operating inside a test.

### Step 3: Google uses special certificates to preserve browser security

Redirecting website traffic creates another technical challenge.

Chrome uses HTTPS certificates to verify that a website is communicating securely with the expected destination.

A local proxy that intercepts traffic could trigger certificate warnings because it is not the original website.

Google addressed this by creating TLS certificates dynamically through a custom certificate authority installed inside the controlled sandbox.

This allows Chrome to validate the simulated websites without displaying security warnings during the test.

Google emphasises that this setup depends on complete control over the isolated testing environment. It is not a technique that an ordinary external attacker can use against a regular Chrome installation without the necessary privileged access.

## AI Agents Build the Fake Websites Automatically

Creating thousands of realistic websites manually would require enormous development resources.

Google therefore built an automated system that uses specialised AI agents to generate and maintain the simulated websites.

The system divides website development into three main roles.

| AI agent role | Responsibility | 
|---|---|
| Research agent | Studies the requested website and prepares specifications | 
| Builder agent | Creates the website’s structure, interface and functionality | 
| Critic agents | Test the website, identify problems and request corrections | 

### Research agents design the websites

The research agent begins with a high-level description of the website or user journey that Google wants to simulate.

For an online shopping platform, it might identify the need for:

- Product categories.
- Search and filtering.
- Shopping cart functionality.
- Discount codes.
- Checkout pages.
- Account-related features.

It then prepares specifications for the builder agent.

### Builder agents create working websites

The builder agent converts these specifications into functional website code.

It generates HTML, CSS and JavaScript to create responsive layouts, navigation systems and interactive elements.

The goal is to produce websites that behave realistically rather than static pages that simply resemble familiar services.

### Critic agents test the results

Critic agents inspect the generated websites and look for problems.

They test accessibility labels, form validation, browser console errors and the behaviour of different page elements.

They can also identify layout problems and request corrections from the builder agent.

This creates an iterative development process in which websites are repeatedly tested and improved.

Google uses Chrome DevTools for Agents to allow these systems to inspect screenshots, click elements, enter information and observe how the websites behave.

## Mirror Dimension Helps Google Test Prompt Injection Attacks

One of the most important purposes of the project is testing how AI agents respond to indirect prompt injection.

Prompt injection occurs when malicious instructions are placed inside content that an AI system reads.

For example, an AI agent might visit a website to collect product information. The page could contain hidden instructions telling the agent to ignore its original task and reveal private information or perform an unauthorised action.

A human visitor may never notice these instructions.

However, an AI agent processing the website’s content could interpret them as commands unless its safeguards correctly distinguish trusted instructions from untrusted information.

This creates a significant security challenge for browser-based AI agents.

### How Google tests hidden instructions

Google can deliberately insert malicious instructions into different parts of its synthetic websites.

These include:

- Visible website text.
- Images containing hidden or misleading instructions.
- HTML accessibility labels.
- WebMCP tool responses.
- Other website elements processed by the AI agent.

The system can also change these malicious payloads during a multi-step task.

This allows researchers to examine whether an agent can recognise and resist manipulation even when the attack appears in an unexpected format.

<AsyncImage query=”AI cybersecurity prompt injection malicious hidden instructions website browser illustration” aspectRatio=”16:9″ width=”100%” maxHeight={250}/>

Google says this approach helps test the protections built into Chrome’s agentic browsing feature, including its ability to identify untrusted instructions and request user confirmation when necessary.

## Google Can Generate More Than 1,000 Security Tests a Day

The scale of the testing system is another important development.

Google says its automated red-teaming pipeline can generate more than 1,000 test candidates every day for each security engineer.

The company estimates that this represents more than 30 times the number of tests an engineer could create manually.

Red teaming involves deliberately testing a system from an attacker’s perspective to discover vulnerabilities before malicious actors can exploit them.

Traditionally, security engineers create attack scenarios, execute them, analyse the results and modify the tests.

The Mirror Dimension automates much of the repetitive work.

Researchers can develop an initial attack idea, while AI agents help generate variations and test them across different websites and user journeys.

| Testing activity | Traditional approach | Mirror Dimension approach | 
|---|---|---|
| Website creation | Significant manual development | AI-assisted generation | 
| Test variation | Primarily manual | Automated generation | 
| Website interaction | Engineer-operated testing | Agents use Chrome tools | 
| Prompt injection | Individually crafted scenarios | Dynamic multi-format payloads | 
| Repeated testing | Manual execution | Automated evaluation pipeline | 

Google says this allows security experts to spend more time discovering new attack techniques rather than repeatedly building and running similar tests.

The 1,000 figure refers to candidate tests generated, not necessarily 1,000 independently confirmed vulnerabilities or completed security fixes.

## Why This Matters as AI Agents Become More Powerful

AI agents are increasingly moving beyond simple conversations.

They can now interact with websites, access connected services, fill out forms and perform multi-step tasks.

This creates new possibilities for productivity.

An AI assistant could research products, organise travel, compare services or help manage online accounts.

However, the same capabilities create security risks.

An agent may encounter malicious website content, misunderstand instructions or attempt actions that the user never intended.

The risk becomes particularly serious when agents have access to sensitive information or tools capable of making purchases, sending messages or modifying accounts.

Google’s Mirror Dimension is designed to help identify these problems before more capable agents are released to larger numbers of users.

It also demonstrates why evaluating AI agents requires more than measuring how accurately they answer questions.

Researchers must examine how they behave when confronted with unexpected information, misleading instructions and potentially harmful requests.

## Google Says the Agents Tested Do Not Have Cyber Capabilities

Google has emphasised that the browsing agents currently being tested inside the Mirror Dimension are benign and do not have cyber capabilities.

The environment is designed to evaluate browser-use behaviour and strengthen the security protections surrounding Chrome’s agentic browsing features.

This distinction matters because the ability to interact with websites does not automatically mean that an AI agent can conduct sophisticated cyberattacks.

Google’s testing environment is intended to identify weaknesses in how agents interpret website content and perform actions.

The company says the Mirror Dimension could eventually become available to more research teams within Google.

It has also encouraged other teams to consider similar isolated environments for evaluating autonomous systems.

## How Mirror Dimension Compares With Other AI Safety Testing Efforts

Google’s project is part of a broader movement towards controlled environments for evaluating autonomous AI.

Governments and technology companies are increasingly recognising that agents require different testing methods from conventional chatbots.

In May 2026, Singapore’s Cyber Security Agency, GovTech and IMDA published findings from an AI Agents Sandbox developed with Google.

That programme explored how computer-use agents could support areas such as quality assurance, AI safety and social assistance.

The work also highlighted risks involving privacy, cybersecurity, human oversight and governance.

The two projects have different purposes, but share an important principle: autonomous agents should be tested in controlled environments before being deployed in sensitive real-world situations.

Google’s Mirror Dimension focuses specifically on browser-use agents and web-based security threats, while the Singapore initiative examined broader agent deployment and governance challenges.

## What Google’s Mirror Dimension Means for India’s AI Industry

The development is relevant to India’s expanding AI ecosystem, where companies are increasingly experimenting with autonomous agents for software development, customer service, research and business automation.

Indian IT services firms and startups could use similar testing environments to evaluate agents before allowing them to interact with enterprise systems.

For example, an AI agent designed to automate customer support may need to navigate websites, retrieve account information and update service requests.

Testing such a system against simulated websites containing malicious instructions could help identify vulnerabilities before deployment.

The approach could also be useful for financial services, healthcare and e-commerce companies, where agents may interact with sensitive customer information.

India’s technology industry has an opportunity to develop its own agent-testing infrastructure and security tools as the adoption of autonomous AI grows.

The broader lesson is that companies should not evaluate agents only on task completion. They must also test whether those systems respect permissions, recognise malicious instructions and ask for human approval when required.

## The Bigger Picture

Google’s Mirror Dimension demonstrates that the next phase of AI development will require increasingly sophisticated testing infrastructure. As agents become capable of browsing websites and completing complex workflows, developers need environments that reproduce the unpredictability of the real internet without exposing real users and businesses to unnecessary risks.

The project also shows how AI can help improve AI safety itself. Google’s specialised agents build websites, test interfaces and generate security scenarios, allowing human engineers to evaluate far more situations than they could manually. However, automated testing is not a guarantee of complete security. New attacks and unexpected agent behaviours will continue to emerge as these systems become more capable.

## Looking Ahead

Google plans to continue improving the realism of the Mirror Dimension and expand access to additional research teams. The company will also need to keep testing its browser agents against new prompt-injection techniques, especially attacks hidden in images, website interfaces and interactive tools. The project’s effectiveness will depend on how closely its simulated environment reflects the changing complexity of the public internet.

For the wider AI industry, the Mirror Dimension could become an important model for evaluating autonomous agents before commercial deployment. As AI assistants gain the ability to operate browsers, applications and digital services, controlled testing environments may become as important as model training itself. The future of agentic AI will depend not only on what these systems can accomplish, but also on whether they can reliably distinguish legitimate instructions from malicious attempts to manipulate them.
