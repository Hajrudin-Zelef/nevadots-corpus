---
id: collect-261121-veille-ia/copilot-microsoft/github-markt-de-puppet-opn-puppet-module-to-manage-opnsense-firewalls-using-their-api-2
title: "GitHub - markt-de/puppet-opn: Puppet module to manage OPNsense firewalls using their API"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["facts", "networking", "example", "opnsense", "puppet", "opnsense01", "resources", "client"]
source: https://github.com/markt-de/puppet-opn
source_anchor: ""
source_lines: [1501, 1597]
section: "Copilot_Microsoft"
---
    'acceptParams' => '0',
  },
}
opn_zabbix_agent_alias { 'ping@opnsense01.example.com':
  ensure => present,
  config => {
    'sourceKey'    => 'icmpping',
    'enabled'      => '1',
    'acceptParams' => '0',
  },
}
```
Exported resources allow application servers (client nodes) to declare OPNsense resources that are collected and applied by the management server. This requires PuppetDB to be set up.

**Client node** — use the `opn::client` class to export resources. Each resource item must include a `devices` key listing the target OPNsense device names. You can use facts like `$facts['networking']['fqdn']` and `$facts['networking']['ip']` in config values to identify the origin node.

```
class { 'opn::client':
  firewall_aliases => {
    'webserver_ips' => {
      'devices' => ['opnsense01.example.com'],
      'config'  => {
        'type'        => 'host',
        'content'     => $facts['networking']['ip'],
        'description' => "${facts['networking']['fqdn']} - Web server IPs",
        'enabled'     => '1',
      },
    },
  },
  haproxy_servers => {
    'web01' => {
      'devices' => ['opnsense01.example.com', 'opnsense02.example.com'],
      'config'  => {
        'address'     => $facts['networking']['ip'],
        'port'        => '8080',
        'description' => "${facts['networking']['fqdn']} - Web backend",
        'enabled'     => '1',
      },
    },
  },
}
```
**Management server** — enable collection with `manage_resources => true` on the `opn` class. Collected resources automatically depend on the per-device credential file.

```
class { 'opn':
  devices => {
    'opnsense01.example.com' => {
      'url'        => 'https://opnsense01.example.com/api',
      'api_key'    => 'OPNSENSE_API_KEY',
      'api_secret' => 'OPNSENSE_API_SECRET',
    },
  },
  manage_resources => true,
}
```
Singleton types (`opn_acmeclient_settings`, `opn_haproxy_settings`, `opn_hasync`, `opn_node_exporter`, `opn_puppet_agent`, `opn_zabbix_agent`, `opn_zabbix_proxy`) are excluded from exported resources because they represent per-device global settings that should only be managed on the management server.

The same parameters can also be configured via Hiera with deep merge:

```
# Client node Hiera data (e.g. role/webserver.yaml)
opn::client::firewall_aliases:
  webserver_ips:
    devices:
      - 'opnsense01.example.com'
    config:
      type: 'host'
      content: "%{facts.networking.ip}"
      description: "%{facts.networking.fqdn} - Web server IPs"
      enabled: '1'
opn::client::haproxy_servers:
  web01:
    devices:
      - 'opnsense01.example.com'
    config:
      address: "%{facts.networking.ip}"
      port: '8080'
      description: "%{facts.networking.fqdn} - Web backend"
      enabled: '1'
```
Migrating to this module is pretty simple: Use the `resource` command to convert the current OPNsense system configuration into Puppet code.

```
$ puppet resource opn_firewall_alias
$ puppet resource opn_zabbix_agent
$ puppet resource opn_zabbix_agent_userparameter
```
Note that this requires a valid puppet-opn configuration to work as expected. Hence enabling this module without additional parameters is the first step.

Classes and parameters are documented in REFERENCE.md.

All default values can be found in the `data/` directory.

Please use the GitHub issues functionality to report any bugs or requests for new features. Feel free to fork and submit pull requests for potential contributions.

BSD-2-Clause
