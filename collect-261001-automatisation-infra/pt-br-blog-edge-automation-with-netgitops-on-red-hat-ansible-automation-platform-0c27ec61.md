Network edge automation challenges
As organizations grow and expand geographi cally, they start extending their IT infrastructure into the distributed and far edge layers through opening new branch offices.
Restaurants, retail stores, and other customer-centric businesses provide differentiated wireless access for their employees, contractors and customers to interconnect within their designated areas.
Configuring and managing multiple wireless settings via Red Hat Ansible Automation Platform simplifies the deployments at scale.
Network administrators can use GitOps practices to automate wireless infrastructure as a code (IaC).
This case covers a sample use case for a company that uses an SDN (software-defined network) controller with a large network infrastructure, including access points, switches, and firewalls/routers to provide connectivity for thousands of branches across multiple countries. We will show you step by step how to automate wireless network access point settings at scale through a SD-WAN controller, which will be Cisco Meraki for purposes of this demo.
Considerations about using a source of control. Why not scripts?
Typically an SDN controller has an API. Having access to an SDN API is an advantage, since we have a single point of contact with the controller, and we can operate the whole network using a single entity and we can interact with it programmatically, leaving behind human intervention.
Does it mean that I have to write a Python script or a program to use it?
Should I learn a programming language in order to use the API?
Short answer: No!
Although learning a programming language is a highly valuable skill, it’s not the fastest or more efficient way to consume the SDN API or solve this problem at enterprise level.
Going through the development path means you’ll end up with a short-term solution that has to be developed, maintained, tested, and documented, with users that must learn how to use it. And frequently, it’s not the best way to use the organization's resources.
Instead, the goal should be to operate the network infrastructure and treat it like code. Describe network elements, its properties and being able to use this description as the source of truth.
What does this mean for our use case (WiFi)?
It means that we describe any required wireless properties, such as credentials and encryption type, and where they should be available (the APs or campuses, device labels).
Therefore we now have two components to consider: First the desired state of the network, and second the logic needed to apply it. We are going to describe how to manage both.
Automating the WiFi network through a SD-WAN controller
We propose using Ansible Automation Platform to handle all the automation process and YAML files to describe (not program!) the tasks needed to achieve the desired state.
The scenario is described by the following diagram:
But how do I trigger the execution of the Ansible Playbooks when I change the desired state? How do I know who made a change? When was it launched?
This is when Ansible Automation Platform comes to help.
Let’s start from the beginning. We need to:
- Describe the wireless network state
- Put this state in a Git repository
- Create a playbook that reads this repository and apply it if needed
Network state
The state is described in YAML below. The specific file for these values will be network_vars.yml.
SSID:
  - name: MyCompany
    number: 2
    enabled: yes
    auth_mode: psk
    encryption_mode: wpa
    psk: Ansible123456!
  - name: MyCompany_employees
    number: 3
    enabled: yes
    auth_mode: psk
    encryption_mode: wpa
    psk: Ansible123456!
  - name: MyCompany_contractors
    number: 4
    enabled: yes
    auth_mode: psk
    encryption_mode: wpa
    psk: Ansible123456!
  - name: MyCompany_customers
    number: 5
    state: absent
    enabled: yes
    auth_mode: psk
    encryption_mode: wpa
    psk: Ansible123456!
WiFi SSIDs playbook:
How do I apply the settings? What will the playbook look like?
Actually, it is quite straightforward.
First, let’s create a playbook file called config_ssids.yml.
Since we’re interacting with the SDN API, in Ansible this means we’re working from localhost.
We also need to read the YAML with values in Ansible to populate a few variables.
So far the playbook looks like this:
---
- hosts: localhost
  vars_files:
    - network_vars.yml
Then we need to use them in the module (cisco.meraki.meraki_mr_ssid). 
    - name: Add SSIDs
      cisco.meraki.meraki_mr_ssid:
        auth_key: "{{ meraki_key }}"
        org_name: "{{ Org_Name }}"
        net_name: "{{ Net_Name }}"
        state: "{{ item.state | default('present') }}"
        name: "{{ item.name }}"
        number: "{{ item.number }}"
        encryption_mode: "{{ item.encryption_mode }}"
        auth_mode: "{{ item.auth_mode }}"
        psk: "{{ item.psk }}" 
      delegate_to: localhost
      loop: "{{ SSID }}"
The final playbook looks like this:
---
- hosts: localhost
  
  vars_files:
    - network_vars.yml
  tasks:
    - name: Add SSIDs
      cisco.meraki.meraki_mr_ssid:
        auth_key: "{{ meraki_key }}"
        org_name: "{{ Org_Name }}"
        net_name: "{{ Net_Name }}"
        state: "{{ item.state | default('present') }}"
        name: "{{ item.name }}"
        number: "{{ item.number }}"
        encryption_mode: "{{ item.encryption_mode }}"
        auth_mode: "{{ item.auth_mode }}"
        psk: "{{ item.psk }}" 
      delegate_to: localhost
      loop: "{{ SSID }}"
Configuring GitHub
Let’s configure GitHub to trigger events from certain Git actions, such as commits and pushes:
First, we initialize our local repository
git  init
Then, we create a GitHub repository (I guess you already have your GitHub account, right?)
We’ll tell our local repo to use this newly created GitHub repo to be the remote repository.
These steps are “suggested” when a new repository is created in GitHub. Anyway, the steps are:
- Remote (github.com):
- Locally, usually from your laptop:
ls -l
total 24
-rw-r--r--  1 pauloseguel  staff    86B Jan 14 16:14 README.md
-rw-r--r--  1 pauloseguel  staff   766B Jan 14 16:14 config_ssids.yml
-rw-------  1 pauloseguel  staff   625B Jan 14 16:14 network_vars.yml
git init
git add README.md network_vars.yml config_ssids.yml
git commit -m "Config SSIDs"
git branch -M main
git remote add origin git@github.com:pseguel-redhat/gitops_test.git
git push -u origin main
Now we need to actually configure how to trigger events from GitHub. The full instructions in Working with Webhooks, and the short version is:
- Configure webhook personal access token (PAT) in GitHub
  - Click on your profile icon, then “Settings”
  - Click on “Developer Settings”, “Personal Access Tokens”
  - “Generate New Token”
  - Add a good description in the “Note” field
  - Automation controller only needs repo scope access, with the exception of invites. Select them.
  - Copy the token before leaving the page, since you won’t be able to access it later.
- Configure from automation controller; the steps are also demonstrated in this video 
 Configuring GitHub PAT Credentials in Ansible Automation Platform, and go as follow: 
  - Create a GitHub personal access token credential type and use the token from above.
- Create a custom credential type for SDN controller:
  - In this case, our playbook is using a single variable (meraki_key ) to store Meraki Dashboard API Token. We’ll use the integrated vault in automation controller to store this value in a secure fashion.
  - Name: Meraki
  - Input configuration:
- In this case, our playbook is using a single variable (
fields:
  - id: MERAKI_KEY
    type: string
    label: Meraki Dashboard API key
    secret: true
- Injector configuration:
extra_vars:
  meraki_key: '{{ MERAKI_KEY }}'
- Create a Meraki credential:
  - Name: Meraki
  - Credential Type: Meraki
  - Meraki Dashboard API key: Token from Meraki Dashboard API
Next, Create a project in automation controller:
- Click on the projects menu item on the left navigation menu.
- Click the blue “Add” button.
- Fill out the following fields:
  - Name: NetGitOps project
  - Source Control Type: Git
  - Source Control URL: https://github.com/pseguel-redhat/gitops_test
    - It should be the one where you host your source of truth
| Important: Enable the option “Update Revision on job launch” | 
- Create a job template
  - Name: Config SSIDs
  - Credentials: Meraki
  - Project: NetGitOps
  - Playbook: config_ssids.yml
  - Webhook: Select the created GitHub PAT credential
  - These steps will create a job template with a webhook key and a webhook URL. They’ll be used in the following GitHub steps.
- Now you have to create a GitHub webhook to integrate with Ansible Automation Platform, detailed steps can be followed also here: 
 
  - Go back to the repo
  - Click on “Settings”
  - Click on “Webhooks”, then “Add webhook”
  - Fill with the following info:
  - Payload URL: The webhook URL from previous step (see image above)
  - Content Type: application/json
  - Secret: Use the webhook key from previous step (see image above)
  - Select the events that trigger the webhook. In our case, we selected just “push”.
  - Leave “Active” checked.
Putting everything together
Let’s have a look at the original state in the SDN dashboard. One SSID is wrong: it should be MyCompany_customers, not MyCompany_clustomers. We need to correct this typo.
This means to modify the network_vars.yml file locally, we must make a commit and push it to the repo. 
The execution can be observed in the video
Triggering Ansible automation from GitHub where we can observe how a job is triggered from GitHub in Ansible Automation Platform, which corresponds to the job template with our playbook and how it changes the environment.
As a final step, let’s go back to the SDN controller dashboard to confirm the changes.
Now the customer’s SSID has a correct name.
What’s next?
At Red Hat, we advocate a “start small, think big” approach for customers that are starting their automation journeys. Some of the most effective beginner tasks are often simpler tasks that are repeated often, and automating them helps users practice automation skills and learn lessons to build up to more complex use cases in the future.
Thinking beyond network configuration automation
We covered the steps required to build a very simple use case for SDN automation through the SD-WAN controller, for a network edge WiFi configuration at scale.
If you are beginning your automation journey, focusing on automating your network configuration tasks is a great starting point to gain consistency, scalability and reduce risk due to human-error. However, configuration automation can only solve a small portion of your IT provisioning workflow.
Frequently, IT processes require approvals, validations, ticket opening/update/closure, and a rollback strategy. There are several improvements or extensions you can make in this scenario:
- Integrate it with an ITSM, such as ServiceNow, to allow you to document every step and change made.
- Trigger a workflow job template, or more complex scenarios, to be executed via webhooks.
- Use automation services catalog in Ansible Automation Platform to launch these tasks with the required approvals.
- Perform verifications before applying changes, as a way to have a configuration drift approach.
If you want to recreate a similar scenario, the code is available in this GitHub repo, and you can sign up for a free trial of Ansible Automation Platform to recreate the steps covered.
Ansible Automation Platform workshops are also available at no-cost to learn how to build and execute network automation in a multi-vendor fashion.
If you are interested in learning more about network automation use cases, we have a good video summary here.
Acknowledgement: I’d like to thank Dafné Mendoza for her outstanding contributions to this blog.
Sobre o autor
Mais como este
O Red Hat Device Edge já está disponível para execução no NVIDIA Jetson Orin
Superando desafios: IA e Kubernetes na fonte dos dados
Untangling Networks | Compiler
Infrastructure At The Edge | Compiler
Navegue por canal
Automação
Últimas novidades em automação de TI para empresas de tecnologia, equipes e ambientes
Inteligência artificial
Descubra as atualizações nas plataformas que proporcionam aos clientes executar suas cargas de trabalho de IA em qualquer ambiente
Nuvem híbrida aberta
Veja como construímos um futuro mais flexível com a nuvem híbrida
Segurança
Veja as últimas novidades sobre como reduzimos riscos em ambientes e tecnologias
Edge computing
Saiba quais são as atualizações nas plataformas que simplificam as operações na borda
Infraestrutura
Saiba o que há de mais recente na plataforma Linux empresarial líder mundial
Aplicações
Conheça nossas soluções desenvolvidas para ajudar você a superar os desafios mais complexos de aplicações
Virtualização
O futuro da virtualização empresarial para suas cargas de trabalho on-premise ou na nuvem