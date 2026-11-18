# Firewallï

The firewall API offers a way for machine to machine interaction between custom applications and OPNsense, it is part of the core system.

Although the module does contains a basic user interface (in ), itâs mirely intended as a reference and testbed. Thereâs no relation to any of the rules being managed via the core system.

Tip

Use your browsers âinspectâ feature to compare requests easily, the user interface in terms of communication is exactly the same as offered by the API . Rules not visible in the web interface () will not be returned by the API either.

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | alias | add_item |  | 
| `POST` | firewall | alias | del_item | $uuid | 
| `GET,POST` | firewall | alias | export |  | 
| `GET` | firewall | alias | get |  | 
| `GET` | firewall | alias | get_alias_u_u_i_d | $name | 
| `GET` | firewall | alias | get_geo_i_p |  | 
| `GET` | firewall | alias | get_item | $uuid=null | 
| `GET` | firewall | alias | get_table_size |  | 
| `POST` | firewall | alias | import |  | 
| `GET` | firewall | alias | list_categories |  | 
| `GET` | firewall | alias | list_countries |  | 
| `GET` | firewall | alias | list_network_aliases |  | 
| `GET` | firewall | alias | list_user_groups |  | 
| `POST` | firewall | alias | reconfigure |  | 
| `GET,POST` | firewall | alias | search_item |  | 
| `POST` | firewall | alias | set |  | 
| `POST` | firewall | alias | set_item | $uuid | 
| `POST` | firewall | alias | toggle_item | $uuid,$enabled=null | 
| `POST` | firewall | alias | update | $action=null | 
| `<<uses>>` |  |  |  | *model* Alias.xml | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | alias_util | add | $alias | 
| `GET` | firewall | alias_util | aliases |  | 
| `POST` | firewall | alias_util | delete | $alias | 
| `POST` | firewall | alias_util | find_references |  | 
| `POST` | firewall | alias_util | flush | $alias | 
| `GET` | firewall | alias_util | list | $alias | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | category | add_item |  | 
| `POST` | firewall | category | del_item | $uuid | 
| `GET` | firewall | category | download |  | 
| `GET` | firewall | category | get |  | 
| `GET` | firewall | category | get_item | $uuid=null | 
| `GET,POST` | firewall | category | search_item | $add_empty=0 | 
| `POST` | firewall | category | set |  | 
| `POST` | firewall | category | set_item | $uuid | 
| `POST` | firewall | category | upload |  | 
| `<<uses>>` |  |  |  | *model* Category.xml | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | d_nat | add_rule |  | 
| `POST` | firewall | d_nat | del_rule | $uuid | 
| `GET` | firewall | d_nat | get_rule | $uuid=null | 
| `GET` | firewall | d_nat | move_rule_before | $selected_uuid,$target_uuid | 
| `GET,POST` | firewall | d_nat | search_rule |  | 
| `POST` | firewall | d_nat | set_rule | $uuid | 
| `POST` | firewall | d_nat | toggle_rule | $uuid,$disabled=null | 
| `GET` | firewall | d_nat | toggle_rule_log | $uuid,$log | 
| `<<uses>>` |  |  |  | *model* DNat.xml | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | filter_base | apply |  | 
| `GET` | firewall | filter_base | get |  | 
| `GET` | firewall | filter_base | list_categories |  | 
| `GET` | firewall | filter_base | list_network_select_options |  | 
| `GET` | firewall | filter_base | list_port_select_options |  | 
| `POST` | firewall | filter_base | set |  | 
| `<<uses>>` |  |  |  | *model* Filter.xml | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | filter | add_rule |  | 
| `POST` | firewall | filter | del_rule | $uuid | 
| `GET` | firewall | filter | download_rules |  | 
| `POST` | firewall | filter | flush_inspect_cache |  | 
| `GET` | firewall | filter | get_interface_list |  | 
| `GET` | firewall | filter | get_rule | $uuid=null | 
| `POST` | firewall | filter | move_rule_before | $selected_uuid,$target_uuid | 
| `GET` | firewall | filter | search_rule |  | 
| `POST` | firewall | filter | set_rule | $uuid | 
| `POST` | firewall | filter | toggle_rule | $uuid,$enabled=null | 
| `GET` | firewall | filter | toggle_rule_log | $uuid,$log | 
| `POST` | firewall | filter | upload_rules |  | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `GET` | firewall | filter_util | rule_stats |  | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | group | add_item |  | 
| `POST` | firewall | group | del_item | $uuid | 
| `GET` | firewall | group | get |  | 
| `GET` | firewall | group | get_item | $uuid=null | 
| `POST` | firewall | group | reconfigure |  | 
| `GET,POST` | firewall | group | search_item |  | 
| `POST` | firewall | group | set |  | 
| `POST` | firewall | group | set_item | $uuid | 
| `<<uses>>` |  |  |  | *model* Group.xml | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `GET` | firewall | migration | download_rules |  | 
| `POST` | firewall | migration | flush |  | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | npt | add_rule |  | 
| `POST` | firewall | npt | del_rule | $uuid | 
| `GET` | firewall | npt | get_rule | $uuid=null | 
| `GET` | firewall | npt | move_rule_before | $selected_uuid,$target_uuid | 
| `GET,POST` | firewall | npt | search_rule |  | 
| `POST` | firewall | npt | set_rule | $uuid | 
| `POST` | firewall | npt | toggle_rule | $uuid,$enabled=null | 
| `GET` | firewall | npt | toggle_rule_log | $uuid,$log | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | one_to_one | add_rule |  | 
| `POST` | firewall | one_to_one | del_rule | $uuid | 
| `GET` | firewall | one_to_one | get_rule | $uuid=null | 
| `GET` | firewall | one_to_one | move_rule_before | $selected_uuid,$target_uuid | 
| `GET,POST` | firewall | one_to_one | search_rule |  | 
| `POST` | firewall | one_to_one | set_rule | $uuid | 
| `POST` | firewall | one_to_one | toggle_rule | $uuid,$enabled=null | 
| `GET` | firewall | one_to_one | toggle_rule_log | $uuid,$log | 

| ï |  |  |  |  | 
|---|---|---|---|---|
| Method | Module | Controller | Command | Parameters | 
|---|---|---|---|---|
| `POST` | firewall | source_nat | add_rule |  | 
| `POST` | firewall | source_nat | del_rule | $uuid | 
| `GET` | firewall | source_nat | get_rule | $uuid=null | 
| `GET` | firewall | source_nat | move_rule_before | $selected_uuid,$target_uuid | 
| `GET,POST` | firewall | source_nat | search_rule |  | 
| `POST` | firewall | source_nat | set_rule | $uuid | 
| `POST` | firewall | source_nat | toggle_rule | $uuid,$enabled=null | 
| `GET` | firewall | source_nat | toggle_rule_log | $uuid,$log | 

## Conceptï

The firewall plugin injects rules in the standard OPNsense firewall while maintaining visibility on them in the standard user interface.

We use our standard `ApiMutableModelControllerBase` to allow crud operations on rule entries and offer an
`apply` action to activate the new configuration.

The diagram above contains the basic steps to change rules and activate them.
Changes made through the administrative endpoints are staged in the configuration; calling `apply()` reloads
the firewall so the new ruleset becomes active.

Note

The examples in this document disable certificate validation, make sure when using this in a production environment to
remove the `verify=False` from the `requests` calls

## Administration exampleï

Administrative endpoints are pretty standard use of `ApiMutableModelControllerBase`, the example below searches for
a rule named âOPNsense_fw_api_testrule_1â, when not found one will be added otherwise it will print the internal uuid.
Inline you will find a brief description of the steps performed.

```
 1#!/usr/bin/env python3.7
 2import requests
 3import json
 4
 5# key + secret from downloaded apikey.txt
 6api_key="3RhWOno+HwvtmT406I6zw8of8J6n9FOKlWK6U0B+K7stt/fDaJg7bjeF3QAshlScYqC+3o5THy3vQViW"
 7api_secret="uaBk27NKhQCZSDpfAlG6YJ473MzvsCNiED6kzbYuykzU05fCRkcJADhDm5nxbZt8yREC74ZpvD/vbcEx"
 8
 9# define the basics, hostname to use and description used to identify our test rule
10rule_description='OPNsense_fw_api_testrule_1'
11remote_uri="https://192.168.1.1"
12
13# search for rule
14r = requests.get(
15    "%s/api/firewall/filter/searchRule?current=1&rowCount=7&searchPhrase=%s" % (
16        remote_uri, rule_description
17    ),
18    auth=(api_key, api_secret), verify=False
19)
20
21if r.status_code == 200:
22    response = json.loads(r.text)
23    if len(response['rows']) == 0:
24        # create a new rule, identified by rule_description allowing traffic from
25        # 192.168.0.0/24 to 10.0.0.0/24 using TCP protocol
26        data = {"rule" :
27                    {
28                    "description": rule_description,
29                    "source_net": "192.168.0.0/24",
30                    "protocol": "TCP",
31                    "destination_net": "10.0.0.0/24"
32                    }
33                }
34        r = requests.post(
35            "%s/api/firewall/filter/addRule" % remote_uri, auth=(api_key, api_secret), verify=False, json=data
36        )
37        if r.status_code == 200:
38            print("created : %s" % json.loads(r.text)['uuid'])
39        else:
40            print("error : %s" % r.text)
41
42    else:
43        for row in response['rows']:
44            print ("found uuid %s" % row['uuid'])
```
Tip

Since our model contains default values for most attributes, we only need to feed the changes if we would like to keep the defaults. In this case the TCP/IP version was IPv4 by default for example. In most cases one would like to set all relevant properties in case defaults change over time.

## Apply exampleï

This example will disable the rule created in the previous example and apply the changes so they become active.

```
 1#!/usr/bin/env python3.7
 2import requests
 3import json
 4
 5# key + secret from downloaded apikey.txt
 6api_key="3RhWOno+HwvtmT406I6zw8of8J6n9FOKlWK6U0B+K7stt/fDaJg7bjeF3QAshlScYqC+3o5THy3vQViW"
 7api_secret="uaBk27NKhQCZSDpfAlG6YJ473MzvsCNiED6kzbYuykzU05fCRkcJADhDm5nxbZt8yREC74ZpvD/vbcEx"
 8
 9# define the basics, hostname to use and description used to identify our test rule
10rule_description='OPNsense_fw_api_testrule_1'
11remote_uri="https://192.168.1.1"
12
13# search for rule
14r = requests.get(
15    "%s/api/firewall/filter/searchRule?current=1&rowCount=7&searchPhrase=%s" % (
16        remote_uri, rule_description
17    ),
18    auth=(api_key, api_secret), verify=False
19)
20
21if r.status_code == 200:
22    response = json.loads(r.text)
23    if len(response['rows']) > 0:
24        rule_uuid = response['rows'][0]['uuid']
25        # disable rule
26        r = requests.post("%s/api/firewall/filter/toggleRule/%s/0" % (remote_uri, rule_uuid),
27                          auth=(api_key, api_secret), verify=False
28        )
29        # apply changes so they become active
30        r = requests.post("%s/api/firewall/filter/apply" % remote_uri,
31                          auth=(api_key, api_secret), verify=False
32        )
33        print("rule %s disabled and applied" % rule_uuid)
34    else:
35        print("rule %s not found" % rule_description)
```