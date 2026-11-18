- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-16-2025 11:59 PM
Hi gang,
I'm working on a full IaC deployment of a Meraki organization using terraform.
I notice some issues with the firewall L3 rule ordering when applying the terraform code.
Since terraform applies all code at the same time unless a dependency is decleared the 10 or so starting rules i have end up in a random order. For the most part i dont care about rule order, but i got some deny rules that must be placed in a spesific sequence.
I notice that the API endpoint for L3 FW rules also does not contain any parameters for sequence.
Has anyone worked around this in a way that is scaleable?
Also if some meraki employees read this, is it possible to add a feature request for firewall sequence numbering?
Thanks in advance!
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Meraki
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 01:15 PM
Can you kindly open an issue on our GitHub repo? Our developers will take a look.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 06:01 AM
You can try using a script (e.g., Python or Bash) that calls the Meraki API directly in the desired order. Terraform can trigger this script using the null_resource provisioner and local-exec.
Please, if this post was useful, leave your kudos and mark it as solved.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 06:13 AM
Question. When using updateNetworkApplianceFirewallL3FirewallRules, the input is an array of rules.
Are you seeing different behavior between Terraform, Postman, Python, etc’?
If so, can you share the Terraform plan you’re using?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 12:07 PM
It is indeed an array.
This is the terraform resource for L3 firewall rules.
resource "meraki_networks_appliance_firewall_l3_firewall_rules" "sb3_fw_l3" {
  network_id = meraki_networks.sb3.id
  rules = [{
    comment        = "Deny-RFC1918."
    dest_cidr      = "10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16"
    dest_port      = "any"
    policy         = "deny"
    protocol       = "any"
    src_cidr       = "any"
    src_port       = "any"
    syslog_enabled = false
  },
  {
    comment        = "Allow-corp-outbound-to-internet."
    dest_cidr      = "any"
    dest_port      = "any"
    policy         = "allow"
    protocol       = "any"
    src_cidr       = var.subnet_prefix_corp
    src_port       = "any"
    syslog_enabled = false
  },
  {
    comment        = "Allow-iot-outbound-to-internet."
    dest_cidr      = "any"
    dest_port      = "any"
    policy         = "allow"
    protocol       = "any"
    src_cidr       = var.subnet_prefix_iot
    src_port       = "any"
    syslog_enabled = false
  },
  {
    comment        = "Allow-guest-outbound-to-internet."
    dest_cidr      = "any"
    dest_port      = "any"
    policy         = "allow"
    protocol       = "any"
    src_cidr       = var.subnet_prefix_guest
    src_port       = "any"
    syslog_enabled = false
  }
  ]
}
This is how it ends up in the Dashboard.
I deleted all rules above and pasted the same array into postman.
Postman body:
{
    "rules": [
        {
            "policy": "deny",
            "protocol": "any",
            "srcCidr": "any",
            "destCidr": "10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16",
            "comment": "Deny-RFC1918.",
            "srcPort": "any",
            "destPort": "any",
            "syslogEnabled": false
        },
        {
            "policy": "allow",
            "protocol": "any",
            "srcCidr": "10.100.2.0/24",
            "destCidr": "any",
            "comment": "Allow-corp-outbound-to-internet.",
            "srcPort": "any",
            "destPort": "any",
            "syslogEnabled": false
        },
        {
            "policy": "allow",
            "protocol": "any",
            "srcCidr": "10.110.2.0/24",
            "destCidr": "any",
            "comment": "Allow-iot-outbound-to-internet.",
            "srcPort": "any",
            "destPort": "any",
            "syslogEnabled": false
        },
        {
            "policy": "allow",
            "protocol": "any",
            "srcCidr": "10.120.2.0/24",
            "destCidr": "any",
            "comment": "Allow-guest-outbound-to-internet.",
            "srcPort": "any",
            "destPort": "any",
            "syslogEnabled": false
        }
    ]
}
Postman response:
200 OK
"rules": [
        {
            "comment": "Deny-RFC1918.",
            "policy": "deny",
            "protocol": "any",
            "srcPort": "Any",
            "srcCidr": "Any",
            "destPort": "Any",
            "destCidr": "10.0.0.0/8,172.16.0.0/12,192.168.0.0/16",
            "syslogEnabled": false
        },
        {
            "comment": "Allow-corp-outbound-to-internet.",
            "policy": "allow",
            "protocol": "any",
            "srcPort": "Any",
            "srcCidr": "10.100.2.0/24",
            "destPort": "Any",
            "destCidr": "Any",
            "syslogEnabled": false
        },
        {
            "comment": "Allow-iot-outbound-to-internet.",
            "policy": "allow",
            "protocol": "any",
            "srcPort": "Any",
            "srcCidr": "10.110.2.0/24",
            "destPort": "Any",
            "destCidr": "Any",
            "syslogEnabled": false
        },
        {
            "comment": "Allow-guest-outbound-to-internet.",
            "policy": "allow",
            "protocol": "any",
            "srcPort": "Any",
            "srcCidr": "10.120.2.0/24",
            "destPort": "Any",
            "destCidr": "Any",
            "syslogEnabled": false
        },
        {
            "comment": "Default rule",
            "policy": "allow",
            "protocol": "Any",
            "srcPort": "Any",
            "srcCidr": "Any",
            "destPort": "Any",
            "destCidr": "Any",
            "syslogEnabled": false
        }
    ]
}
Dashboard:
Everything is identical in the array. The only difference is how i populate the fields in terraform. I'm just pointing to some variables defined in variables.tf
Here they are:
variable "subnet_prefix_corp" {
    type = string
    default = "10.100.2.0/24"
}
variable "appliance_ip_corp" {
    type = string
    default = "10.100.2.1"
}
variable "subnet_prefix_iot" {
    type = string
    default = "10.110.2.0/24"
}
variable "appliance_ip_iot" {
    type = string
    default = "10.110.2.1"
}
variable "subnet_prefix_guest" {
    type = string
    default = "10.120.2.0/24"
}
variable "appliance_ip_guest" {
    type = string
    default = "10.120.2.1"
}
variable "subnet_prefix_mgmt" {
    type = string
    default = "10.130.2.0/24"
}
variable "appliance_ip_mgmt" {
    type = string
    default = "10.130.2.1"
}
It looks like Terraform is somehow re-ordering the array. But i'm not sure how or why
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 12:37 PM
Wrap your rules in a tolist() to force Terraform to treat it as an ordered list.
Please, if this post was useful, leave your kudos and mark it as solved.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 01:10 PM
Thanks for the tip, but unfortunately it did not solve the issue.
rules wrapped in a tolist using locals:
locals {
  firewall_rules = tolist([
    {
      comment        = "Deny-RFC1918."
      dest_cidr      = "10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16"
      dest_port      = "any"
      policy         = "deny"
      protocol       = "any"
      src_cidr       = "any"
      src_port       = "any"
      syslog_enabled = false
    },
    {
      comment        = "Allow-corp-outbound-to-internet."
      dest_cidr      = "any"
      dest_port      = "any"
      policy         = "allow"
      protocol       = "any"
      src_cidr       = var.subnet_prefix_corp
      src_port       = "any"
      syslog_enabled = false
    },
    {
      comment        = "Allow-iot-outbound-to-internet."
      dest_cidr      = "any"
      dest_port      = "any"
      policy         = "allow"
      protocol       = "any"
      src_cidr       = var.subnet_prefix_iot
      src_port       = "any"
      syslog_enabled = false
    },
    {
      comment        = "Allow-guest-outbound-to-internet."
      dest_cidr      = "any"
      dest_port      = "any"
      policy         = "allow"
      protocol       = "any"
      src_cidr       = var.subnet_prefix_guest
      src_port       = "any"
      syslog_enabled = false
    }
  ])
}
resource "meraki_networks_appliance_firewall_l3_firewall_rules" "sb3_fw_l3" {
  network_id = meraki_networks.sb3.id
  rules      = local.firewall_rules
}
Dashboard:
Copy of the state file block for firewall rules.
{
      "mode": "managed",
      "type": "meraki_networks_appliance_firewall_l3_firewall_rules",
      "name": "sb3_fw_l3",
      "provider": "provider[\"registry.terraform.io/cisco-open/meraki\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "network_id": "xxxxxxxxxx",
            "rules": [
              {
                "comment": "Allow-corp-outbound-to-internet.",
                "dest_cidr": "any",
                "dest_port": "any",
                "policy": "allow",
                "protocol": "any",
                "src_cidr": "10.100.2.0/24",
                "src_port": "any",
                "syslog_enabled": false
              },
              {
                "comment": "Allow-guest-outbound-to-internet.",
                "dest_cidr": "any",
                "dest_port": "any",
                "policy": "allow",
                "protocol": "any",
                "src_cidr": "10.120.2.0/24",
                "src_port": "any",
                "syslog_enabled": false
              },
              {
                "comment": "Allow-iot-outbound-to-internet.",
                "dest_cidr": "any",
                "dest_port": "any",
                "policy": "allow",
                "protocol": "any",
                "src_cidr": "10.110.2.0/24",
                "src_port": "any",
                "syslog_enabled": false
              },
              {
                "comment": "Deny-RFC1918.",
                "dest_cidr": "10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16",
                "dest_port": "any",
                "policy": "deny",
                "protocol": "any",
                "src_cidr": "any",
                "src_port": "any",
                "syslog_enabled": false
              }
            ],
            "rules_response": [
              {
                "comment": "Allow-corp-outbound-to-internet.",
                "dest_cidr": "Any",
                "dest_port": "Any",
                "policy": "allow",
                "protocol": "any",
                "src_cidr": "10.100.2.0/24",
                "src_port": "Any",
                "syslog_enabled": false
              },
              {
                "comment": "Allow-guest-outbound-to-internet.",
                "dest_cidr": "Any",
                "dest_port": "Any",
                "policy": "allow",
                "protocol": "any",
                "src_cidr": "10.120.2.0/24",
                "src_port": "Any",
                "syslog_enabled": false
              },
              {
                "comment": "Allow-iot-outbound-to-internet.",
                "dest_cidr": "Any",
                "dest_port": "Any",
                "policy": "allow",
                "protocol": "any",
                "src_cidr": "10.110.2.0/24",
                "src_port": "Any",
                "syslog_enabled": false
              },
              {
                "comment": "Default rule",
                "dest_cidr": "Any",
                "dest_port": "Any",
                "policy": "allow",
                "protocol": "Any",
                "src_cidr": "Any",
                "src_port": "Any",
                "syslog_enabled": false
              },
              {
                "comment": "Deny-RFC1918.",
                "dest_cidr": "10.0.0.0/8,172.16.0.0/12,192.168.0.0/16",
                "dest_port": "Any",
                "policy": "deny",
                "protocol": "any",
                "src_cidr": "Any",
                "src_port": "Any",
                "syslog_enabled": false
              }
            ],
            "syslog_default_rule": null
          },
          "sensitive_attributes": [],
          "identity_schema_version": 0,
          "dependencies": [
            "meraki_networks.sb3"
          ]
        }
      ]
    }
The order in the statefile matches what i see in the dashboard. I just dont get why it reorders the array.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 01:15 PM
Can you kindly open an issue on our GitHub repo? Our developers will take a look.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 01:16 PM
With the debug information, please.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-17-2025 01:42 PM
Done. Here is a link to the issue.
Thanks for the replies 
https://github.com/cisco-open/terraform-provider-meraki/issues/274
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-21-2025 12:12 PM
Fyi there is a commit do the dev branch of the repo with a possible solution now.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-21-2025 01:54 PM
And now its merged with main. New provider version 1.1.7-beta is out
Thanks everyone!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-21-2025 03:47 PM
Happy coding!