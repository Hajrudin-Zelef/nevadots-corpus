config webfilter profile
config webfilter profile
Configure Web filter profiles.
config webfilter profile
    Description: Configure Web filter profiles.
    edit <name>
        config antiphish
            Description: AntiPhishing profile.
            set status [enable|disable]
            set default-action [exempt|log|...]
            set check-uri [enable|disable]
            set check-basic-auth [enable|disable]
            set check-username-only [enable|disable]
            set max-body-len {integer}
            config inspection-entries
                Description: AntiPhishing entries.
                edit <name>
                    set fortiguard-category {user}
                    set action [exempt|log|...]
                next
            end
            config custom-patterns
                Description: Custom username and password regex patterns.
                edit <pattern>
                    set category [username|password]
                    set type [regex|literal]
                next
            end
            set authentication [domain-controller|ldap]
            set domain-controller {string}
            set ldap {string}
        end
        set comment {var-string}
        set extended-log [enable|disable]
        set feature-set [flow|proxy]
        config ftgd-wf
            Description: FortiGuard Web Filter settings.
            set options {option1}, {option2}, ...
            set exempt-quota {user}
            set ovrd {user}
            config filters
                Description: FortiGuard filters.
                edit <id>
                    set category {integer}
                    set action [block|authenticate|...]
                    set warn-duration {user}
                    set auth-usr-grp <name1>, <name2>, ...
                    set log [enable|disable]
                    set override-replacemsg {string}
                    set warning-prompt [per-domain|per-category]
                    set warning-duration-type [session|timeout]
                next
            end
            config quota
                Description: FortiGuard traffic quota settings.
                edit <id>
                    set category {user}
                    set type [time|traffic]
                    set unit [B|KB|...]
                    set value {integer}
                    set duration {user}
                    set override-replacemsg {string}
                next
            end
            set max-quota-timeout {integer}
            set rate-javascript-urls [disable|enable]
            set rate-css-urls [disable|enable]
            set rate-crl-urls [disable|enable]
        end
        set https-replacemsg [enable|disable]
        set log-all-url [enable|disable]
        set options {option1}, {option2}, ...
        config override
            Description: Web Filter override settings.
            set ovrd-cookie [allow|deny]
            set ovrd-scope [user|user-group|...]
            set profile-type [list|radius]
            set ovrd-dur-mode [constant|ask]
            set ovrd-dur {user}
            set profile-attribute [User-Name|NAS-IP-Address|...]
            set ovrd-user-group <name1>, <name2>, ...
            set profile <name1>, <name2>, ...
        end
        set ovrd-perm {option1}, {option2}, ...
        set post-action [normal|block]
        set replacemsg-group {string}
        config web
            Description: Web content filtering settings.
            set bword-threshold {integer}
            set bword-table {integer}
            set urlfilter-table {integer}
            set content-header-list {integer}
            set blocklist [enable|disable]
            set allowlist {option1}, {option2}, ...
            set safe-search {option1}, {option2}, ...
            set youtube-restrict [none|strict|...]
            set vimeo-restrict {string}
            set log-search [enable|disable]
            set keyword-match <pattern1>, <pattern2>, ...
        end
        set web-antiphishing-log [enable|disable]
        set web-content-log [enable|disable]
        set web-extended-all-action-log [enable|disable]
        set web-filter-activex-log [enable|disable]
        set web-filter-applet-log [enable|disable]
        set web-filter-command-block-log [enable|disable]
        set web-filter-cookie-log [enable|disable]
        set web-filter-cookie-removal-log [enable|disable]
        set web-filter-js-log [enable|disable]
        set web-filter-jscript-log [enable|disable]
        set web-filter-referer-log [enable|disable]
        set web-filter-unknown-log [enable|disable]
        set web-filter-vbs-log [enable|disable]
        set web-ftgd-err-log [enable|disable]
        set web-ftgd-quota-usage [enable|disable]
        set web-invalid-domain-log [enable|disable]
        set web-url-log [enable|disable]
        set wisp [enable|disable]
        set wisp-algorithm [primary-secondary|round-robin|...]
        set wisp-servers <name1>, <name2>, ...
    next
end
                                            config webfilter profile
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| comment | Optional comments. | var-string | Maximum length: 255 |  | 
| extended-log | Enable/disable extended logging for web filtering. | option | - | disable | 
|  |  |  |  |  | 
| feature-set | Flow/proxy feature set. | option | - | flow | 
|  |  |  |  |  | 
| https-replacemsg | Enable replacement messages for HTTPS. | option | - | enable | 
|  |  |  |  |  | 
| log-all-url | Enable/disable logging all URLs visited. | option | - | disable | 
|  |  |  |  |  | 
| name | Profile name. | string | Maximum length: 35 |  | 
| options | Options. | option | - |  | 
|  |  |  |  |  | 
| ovrd-perm | Permitted override types. | option | - |  | 
|  |  |  |  |  | 
| post-action | Action taken for HTTP POST traffic. | option | - | normal | 
|  |  |  |  |  | 
| replacemsg-group | Replacement message group. | string | Maximum length: 35 |  | 
| web-antiphishing-log | Enable/disable logging of AntiPhishing checks. | option | - | enable | 
|  |  |  |  |  | 
| web-content-log | Enable/disable logging logging blocked web content. | option | - | enable | 
|  |  |  |  |  | 
| web-extended-all-action-log | Enable/disable extended any filter action logging for web filtering. | option | - | disable | 
|  |  |  |  |  | 
| web-filter-activex-log | Enable/disable logging ActiveX. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-applet-log | Enable/disable logging Java applets. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-command-block-log | Enable/disable logging blocked commands. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-cookie-log | Enable/disable logging cookie filtering. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-cookie-removal-log | Enable/disable logging blocked cookies. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-js-log | Enable/disable logging Java scripts. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-jscript-log | Enable/disable logging JScripts. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-referer-log | Enable/disable logging referrers. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-unknown-log | Enable/disable logging unknown scripts. | option | - | enable | 
|  |  |  |  |  | 
| web-filter-vbs-log | Enable/disable logging VBS scripts. | option | - | enable | 
|  |  |  |  |  | 
| web-ftgd-err-log | Enable/disable logging rating errors. | option | - | enable | 
|  |  |  |  |  | 
| web-ftgd-quota-usage | Enable/disable logging daily quota usage. | option | - | enable | 
|  |  |  |  |  | 
| web-invalid-domain-log | Enable/disable logging invalid domain names. | option | - | enable | 
|  |  |  |  |  | 
| web-url-log | Enable/disable logging URL filtering. | option | - | enable | 
|  |  |  |  |  | 
| wisp | Enable/disable web proxy WISP. | option | - | disable | 
|  |  |  |  |  | 
| wisp-algorithm | WISP server selection algorithm. | option | - | auto-learning | 
|  |  |  |  |  | 
| wisp-servers <name> | WISP servers. Server name. | string | Maximum length: 79 |  | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| flow | Flow feature set. | 
| proxy | Proxy feature set. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| activexfilter | ActiveX filter. | 
| cookiefilter | Cookie filter. | 
| javafilter | Java applet filter. | 
| block-invalid-url | Block sessions contained an invalid domain name. | 
| jscript | Javascript block. | 
| js | JS block. | 
| vbs | VB script block. | 
| unknown | Unknown script block. | 
| intrinsic | Intrinsic script block. | 
| wf-referer | Referring block. | 
| wf-cookie | Cookie block. | 
| per-user-bal | Per-user block/allow list filter | 
| Option | Description | 
|---|---|
| bannedword-override | Banned word override. | 
| urlfilter-override | URL filter override. | 
| fortiguard-wf-override | FortiGuard Web Filter override. | 
| contenttype-check-override | Content-type header override. | 
| Option | Description | 
|---|---|
| normal | Normal, POST requests are allowed. | 
| block | POST requests are blocked. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable web proxy WISP. | 
| disable | Disable web proxy WISP. | 
| Option | Description | 
|---|---|
| primary-secondary | Select the first healthy server in order. | 
| round-robin | Select the next healthy server. | 
| auto-learning | Select the lightest loading healthy server. | 
config antiphish
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| status | Toggle AntiPhishing functionality. | option | - | disable | 
|  |  |  |  |  | 
| default-action | Action to be taken when there is no matching rule. | option | - | exempt | 
|  |  |  |  |  | 
| check-uri | Enable/disable checking of GET URI parameters for known credentials. | option | - | disable | 
|  |  |  |  |  | 
| check-basic-auth | Enable/disable checking of HTTP Basic Auth field for known credentials. | option | - | disable | 
|  |  |  |  |  | 
| check-username-only | Enable/disable username only matching of credentials. Action will be taken for valid usernames regardless of password validity. | option | - | disable | 
|  |  |  |  |  | 
| max-body-len | Maximum size of a POST body to check for credentials. | integer | Minimum value: 0 Maximum value: 4294967295 | 65536 | 
| authentication | Authentication methods. | option | - | domain-controller | 
|  |  |  |  |  | 
| domain-controller | Domain for which to verify received credentials against. | string | Maximum length: 63 |  | 
| ldap | LDAP server for which to verify received credentials against. | string | Maximum length: 63 |  | 
| Option | Description | 
|---|---|
| enable | Enable AntiPhishing functionality. | 
| disable | Disable AntiPhishing functionality. | 
| Option | Description | 
|---|---|
| exempt | Exempt requests from matching. | 
| log | Log all matched requests. | 
| block | Block all matched requests. | 
| Option | Description | 
|---|---|
| enable | Enable checking of GET URI for username and password fields. | 
| disable | Disable checking of GET URI for username and password fields. | 
| Option | Description | 
|---|---|
| enable | Enable checking of HTTP Basic Auth field for known credentials. | 
| disable | Disable checking of HTTP Basic Auth field for known credentials. | 
| Option | Description | 
|---|---|
| enable | Enable username only credential matches. | 
| disable | Disable username only credential matches. | 
| Option | Description | 
|---|---|
| domain-controller | Domain Controller to verify user credential. | 
| ldap | LDAP to verify user credential. | 
config inspection-entries
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| name | Inspection target name. | string | Maximum length: 63 |  | 
| fortiguard-category | FortiGuard category to match. | user | Not Specified | 0 | 
| action | Action to be taken upon an AntiPhishing match. | option | - | exempt | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| exempt | Exempt requests from matching. | 
| log | Log all matched requests. | 
| block | Block all matched requests. | 
config custom-patterns
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| pattern | Target pattern. | string | Maximum length: 255 |  | 
| category | Category that the pattern matches. | option | - | username | 
|  |  |  |  |  | 
| type | Pattern will be treated either as a regex pattern or literal string. | option | - | regex | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| username | Pattern matches username fields. | 
| password | Pattern matches password fields. | 
| Option | Description | 
|---|---|
| regex | Pattern will be treated as a regex pattern. | 
| literal | Pattern will be treated as a literal string. | 
config ftgd-wf
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| options | Options for FortiGuard Web Filter. | option | - | ftgd-disable | 
|  |  |  |  |  | 
| exempt-quota | Do not stop quota for these categories. | user | Not Specified | 17 | 
| ovrd | Allow web filter profile overrides. | user | Not Specified |  | 
| max-quota-timeout | Maximum FortiGuard quota used by single page view in seconds (excludes streams). | integer | Minimum value: 1 Maximum value: 86400 | 300 | 
| rate-javascript-urls | Enable/disable rating JavaScript by URL. | option | - | enable | 
|  |  |  |  |  | 
| rate-css-urls | Enable/disable rating CSS by URL. | option | - | enable | 
|  |  |  |  |  | 
| rate-crl-urls | Enable/disable rating CRL by URL. | option | - | enable | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| error-allow | Allow web pages with a rating error to pass through. | 
| rate-server-ip | Rate the server IP in addition to the domain name. | 
| connect-request-bypass | Bypass connection which has CONNECT request. | 
| ftgd-disable | Disable FortiGuard scanning. | 
| Option | Description | 
|---|---|
| disable | Disable rating JavaScript by URL. | 
| enable | Enable rating JavaScript by URL. | 
| Option | Description | 
|---|---|
| disable | Disable rating CSS by URL. | 
| enable | Enable rating CSS by URL. | 
| Option | Description | 
|---|---|
| disable | Disable rating CRL by URL. | 
| enable | Enable rating CRL by URL. | 
config filters
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| id | ID number. | integer | Minimum value: 0 Maximum value: 255 | 0 | 
| category | Categories and groups the filter examines. | integer | Minimum value: 0 Maximum value: 255 | 0 | 
| action | Action to take for matches. | option | - | monitor | 
|  |  |  |  |  | 
| warn-duration | Duration of warnings. | user | Not Specified | 5m | 
| auth-usr-grp <name> | Groups with permission to authenticate. User group name. | string | Maximum length: 79 |  | 
| log | Enable/disable logging. | option | - | enable | 
|  |  |  |  |  | 
| override-replacemsg | Override replacement message. | string | Maximum length: 28 |  | 
| warning-prompt | Warning prompts in each category or each domain. | option | - | per-category | 
|  |  |  |  |  | 
| warning-duration-type | Re-display warning after closing browser or after a timeout. | option | - | timeout | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| block | Block access. | 
| authenticate | Authenticate user before allowing access. | 
| monitor | Allow access while logging the action. | 
| warning | Allow access after warning the user. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| per-domain | Per-domain warnings. | 
| per-category | Per-category warnings. | 
| Option | Description | 
|---|---|
| session | After session ends. | 
| timeout | After timeout occurs. | 
config quota
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| id | ID number. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| category | FortiGuard categories to apply quota to (category action must be set to monitor). | user | Not Specified |  | 
| type | Quota type. | option | - | time | 
|  |  |  |  |  | 
| unit | Traffic quota unit of measurement. | option | - | MB | 
|  |  |  |  |  | 
| value | Traffic quota value. | integer | Minimum value: 1 Maximum value: 4294967295 | 1024 | 
| duration | Duration of quota. | user | Not Specified | 5m | 
| override-replacemsg | Override replacement message. | string | Maximum length: 28 |  | 
| Option | Description | 
|---|---|
| time | Use a time-based quota. | 
| traffic | Use a traffic-based quota. | 
| Option | Description | 
|---|---|
| B | Quota in bytes. | 
| KB | Quota in kilobytes. | 
| MB | Quota in megabytes. | 
| GB | Quota in gigabytes. | 
config override
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| ovrd-cookie | Allow/deny browser-based (cookie) overrides. | option | - | deny | 
|  |  |  |  |  | 
| ovrd-scope | Override scope. | option | - | user | 
|  |  |  |  |  | 
| profile-type | Override profile type. | option | - | list | 
|  |  |  |  |  | 
| ovrd-dur-mode | Override duration mode. | option | - | constant | 
|  |  |  |  |  | 
| ovrd-dur | Override duration. | user | Not Specified | 15m | 
| profile-attribute | Profile attribute to retrieve from the RADIUS server. | option | - | Login-LAT-Service | 
|  |  |  |  |  | 
| ovrd-user-group <name> | User groups with permission to use the override. User group name. | string | Maximum length: 79 |  | 
| profile <name> | Web filter profile with permission to create overrides. Web profile. | string | Maximum length: 79 |  | 
| Option | Description | 
|---|---|
| allow | Allow browser-based (cookie) override. | 
| deny | Deny browser-based (cookie) override. | 
| Option | Description | 
|---|---|
| user | Override for the user. | 
| user-group | Override for the user's group. | 
| ip | Override for the initiating IP. | 
| browser | Create browser-based (cookie) override. | 
| ask | Prompt for scope when initiating an override. | 
| Option | Description | 
|---|---|
| list | Profile chosen from list. | 
| radius | Profile determined by RADIUS server. | 
| Option | Description | 
|---|---|
| constant | Constant mode. | 
| ask | Prompt for duration when initiating an override. | 
| Option | Description | 
|---|---|
| User-Name | Use this attribute. | 
| NAS-IP-Address | Use this attribute. | 
| Framed-IP-Address | Use this attribute. | 
| Framed-IP-Netmask | Use this attribute. | 
| Filter-Id | Use this attribute. | 
| Login-IP-Host | Use this attribute. | 
| Reply-Message | Use this attribute. | 
| Callback-Number | Use this attribute. | 
| Callback-Id | Use this attribute. | 
| Framed-Route | Use this attribute. | 
| Framed-IPX-Network | Use this attribute. | 
| Class | Use this attribute. | 
| Called-Station-Id | Use this attribute. | 
| Calling-Station-Id | Use this attribute. | 
| NAS-Identifier | Use this attribute. | 
| Proxy-State | Use this attribute. | 
| Login-LAT-Service | Use this attribute. | 
| Login-LAT-Node | Use this attribute. | 
| Login-LAT-Group | Use this attribute. | 
| Framed-AppleTalk-Zone | Use this attribute. | 
| Acct-Session-Id | Use this attribute. | 
| Acct-Multi-Session-Id | Use this attribute. | 
config web
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| bword-threshold | Banned word score threshold. | integer | Minimum value: 0 Maximum value: 2147483647 | 10 | 
| bword-table | Banned word table ID. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| urlfilter-table | URL filter table ID. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| content-header-list | Content header list. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| blocklist | Enable/disable automatic addition of URLs detected by FortiSandbox to blocklist. | option | - | disable | 
|  |  |  |  |  | 
| allowlist | FortiGuard allowlist settings. | option | - |  | 
|  |  |  |  |  | 
| safe-search | Safe search type. | option | - |  | 
|  |  |  |  |  | 
| youtube-restrict | YouTube EDU filter level. | option | - | none | 
|  |  |  |  |  | 
| vimeo-restrict | Set Vimeo-restrict ("7" = don't show mature content, "134" = don't show unrated and mature content). A value of cookie "content_rating". | string | Maximum length: 63 |  | 
| log-search | Enable/disable logging all search phrases. | option | - | disable | 
|  |  |  |  |  | 
| keyword-match <pattern> | Search keywords to log when match is found. Pattern/keyword to search for. | string | Maximum length: 79 | ** | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| exempt-av | Exempt antivirus. | 
| exempt-webcontent | Exempt web content. | 
| exempt-activex-java-cookie | Exempt ActiveX-JAVA-Cookie. | 
| exempt-dlp | Exempt DLP. | 
| exempt-rangeblock | Exempt RangeBlock. | 
| extended-log-others | Support extended log. | 
| Option | Description | 
|---|---|
| url | Insert safe search string into URL. | 
| header | Insert safe search header. | 
| Option | Description | 
|---|---|
| none | Full access for YouTube. | 
| strict | Strict access for YouTube. | 
| moderate | Moderate access for YouTube. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. |