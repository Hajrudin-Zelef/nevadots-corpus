## Important facts to know

Main parts of the Web Filtering in Fortigate:

- 
**SSL Profile** - either Certificate-only or Deep SSL Inspection, tells Fortigate whether to decrypt completely SSL communication or look just at domain names in the SSL Certificates. The*no-inspection* profile disables SSL inspection altogether, meaning any HTTPS websites will not be scanned.
- 
**FortiGuard Web Filtering** service - enables us to filter web sites/URLs by Category, instead of static URLs. You need Web Filter license for that.
- 
**Static URL Filter** - checks URL of the web site a user tries to enter, can be used together with Fortiguard Web Filtering.
- 
**Web Content Filtering** - looks inside the**Contents** of a web page to find predefined by us "banned" words and makes decision based on their existence and their count on the page.

Order of processing:

1. 
Static URL Filter
2. 
Fortiguard Category Filter (Local/custom Categories → Remote/external feed Category → FortiGuard Category)
3. 
Web Content Filter
4. 
Advanced options filter - proxy mode only (ActiveX, Java Applets etc.)
5. 
AntiVirus Scanning

**Work Flow** - Fine tune SSL Profile if built-ins are not enough, create webfilter profile in Security Profiles, fine tune it, apply to Security rule, together with SSL profile and enable UTM in such rule.

| Note | Web Filter-related databases on Fortigate are not updated by default, only after you create 1st Web Filter Profile and use it in the security rule, will Fortigate update dbs. | 

The yet-to-be-configured Web Filter Profile will look like:

## Static URL Filter

With this filter we create entries in the filter list, each matching a URL/domain together with the desired action. The list is processed in top to down order, 1st match stops further processing.

The actions:

- 
**Block** - block connection, no other processing (by AV/IPS signatures/etc.) is done. User sees custom or default block page that access was blocked by the policy.
- 
**Allow** - allow connection from URL Filtering point of view, the connection will still  be checked by other security profiles/features if available - IPS/AppControl/AV, etc. This includes FortiGuard/Category based filtering - if Static URL is set to Allow this URL, but in Category it is in the blocked category - the connection will be blocked.
- 
**Exempt** - allow connection AND stop any further security checks like AV/AppControl/IPS. This will exempt the connection even from Category based URL filtering. We can decide what exact further checks to exempt, see below.
- 
**Monitor** - monitor (and log if logs are enabled in the security rule) but do NOT block the connection.

In Static URL filter, we match domain/URL by either **Simple**, **Wildcard**, or **Regex** expressions.

**Simple** is what it says - matches exactly what you put in it. It is the least flexible but also least resource-intensive for the Fortigate. Also, its matching depends on the inspection mode - Proxy or Flow. In proxy the match is exact - e.g. if we set as Simple "yurisk.com" , it will match  just yurisk.com, but not anything else, like any subdomain - www.yurisk.com, test.yurisk.com will NOT be matched. In the Flow mode, it may also do partial matching, including sub-domains.

We may indicate the protocol as well - HTTP or HTTPS, but Fortigate will remove it anyway.

The whole URLs can be matched as well - if I want to block only say a specific page https://yurisk.info/2020/05/20/fortigate-bgp-cookbook-of-example-configuration-and-debug/ but allow anything else in yurisk.info, this will work too:

- 
CLI configuration: create static URL list, then reference it by its table number in the webfilter profile:

```
config webfilter urlfilter
    edit 1   <-- TABLE ID TO BE REFERENCED IN PROFILE
        set name "Auto-webfilter-urlfilter_12gn7p0os"
        config entries
            edit 2
                set url "yurisk.com"
                set action block
            next
            edit 3
                set url "yurisk.info/2020/05/20/
                fortigate-bgp-cookbook-of-example-configuration-and-debug/"
                set action block
            next
        end
    next
end
```
```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        config web
            set urlfilter-table 1 
```
**(1)**
        end
1. 
Number 1 is a reference to the URL filter table entry, this way (and only in CLI) we can reference the same URLs table in different web filtering profiles, so not to re-create the same table of URLs for each new profile.

**Wildcard** match - does what the names says, we can replace any part of the domain/URL with * . When using it in the form of *.example.com, this will match all subdomains of the example.com  as well as the root domain example.com itself. Some examples: `*facebook.com`, `forti*.com`

**Regex** matching - Fortinet uses PCRE standard regex syntax. Few notes:

- 
You do not have to escape dot "." used to separate domain name parts, e.g. www.example.com is OK, but "proper" www\.example\.com would work too.
- 
By default, the regex is NOT case-sensitive, but if for some strange reason you need it to be - add after the regex "i" option, e.g. /EXAMPLE.COM/i and it will only match EXAMPLE.COM, not example.com.
- 
We can use "^" to anchor the regex to the beginning of an domain/URL string.
- 
When entering regex on CLI, escape any special symbol twice, see below

E.g. of `urlfilter`  in GUI and on CLI:

CLI:

```
config webfilter urlfilter
    edit 1
        set name "Auto-webfilter-urlfilter_12gn7p0os"
        config entries
            edit 9
                set url "yurisk.info"
            next
            edit 2
                set url "*.yurisk.com"
                set type wildcard
                set action block
            next
            edit 3
                set url "yurisk.info/2020/05/20/fortigate-bgp-cookbook-of-example-configuration-and-debug/"
                set action block
            next
            edit 4
                set url "espn.com"
                set exempt fortiguard
            next
            edit 5
                set url "*.msn.com"
                set type wildcard
                set action block
            next
            edit 6
                set url "stackoverflow.com"
                set type regex
                set action block
            next
            edit 7
                set url "sky\\.com"
                set type regex
                set action block
            next
        end
end
```
**Exempt** - this action allows the stated URL and skips any or specific further processing. The option to specify what further checks to skip is available on CLI only. E.g. here I exempt "espn.com" only from FortiGuard checks, but not from AV or other checks if they are set via Security Profiles in the security rule. As I have the category "Sports" inside Fortigaurd Category filtering set to action Authenticate, this exempt will disable Authentication when entering "espn.com" (and any URL path), but will leave Authentication enabled for any other website in the Sports category, including say espn.co.uk.

CLI config:

```
config webfilter urlfilter
    edit 1
        set name "Auto-webfilter-urlfilter_12gn7p0os"
        config entries
                    edit 4
                set url "espn.com"
                set exempt fortiguard
            next
        end
    next
end
```
Besides `fortiguard` we can exempt any of the below:

 (0) # set exempt
av                     AntiVirus scanning.
web-content            Web filter content matching.
activex-java-cookie    ActiveX, Java, and cookie filtering.
dlp                    DLP scanning.
fortiguard             FortiGuard web filtering.
range-block            Range block feature.
pass                   Pass single connection from all.
antiphish              AntiPhish credential checking.
all                    Exempt from all security profiles. <-- DEFAULT IN GUI

The result of the above configuration will look in GUI as:

In logs this Exempt will appear as "Passthrough":

## FortiGuard Category based Web filtering

For this filtering, the Fortigate on each web site request by users (responses are cached) queries the FortiGuard Distribution Network (FDN). The FDN collects billions of web sites categorizing them, by both domain names and IP addresses.

Prerequisites:

- 
Valid Web Filtering license, see in Dashboard or **dia debug rating**
- 
Being able to connect to FortiGuard servers, again start with **dia debug rating** to verify.
- 
As an option, Fortimanager that does have access to Fortiguard can be configured as FDS to be used by Fortigates.
- 
You have to use SSL inspection Profile in security rule. When using Certitificate-only profile, the Fortigate will only be able to check **domain name** of the website, not page contents, nor the full path in URL (part after the 1st slash in URL). For maximum efficiency, the Deep SSL Inspection SSL profile should be used (but beware of browser error on MiTM - have to 1st install the Fortigate CA certificate on all end stations).
- 
Watch out for **SSL Exemption** list inside the SSL Inspection Profile - categories/domains/IP addresses listed there will NOT be inspected if they use SSL (most of the websites) and thus features like Usage Quota/Warning/etc. will work but in a bit different way. See Usage Quota.

| Tip | To see category of a domain, you either check Fortiguard site https://www.fortiguard.com/webfilter or can go to Security Profiles → Web Rating Overrides → Select Create New → URL put the domain in question and click "Lookup Rating" | 

To list all categories on CLI: **get webfilter categories**.

Configuring it is simple - just enable it in Web Filter Profile as Category Based Filter, choose Categories and their Action and use this Web Filter in security rule(s) with UTM enabled.

- 
**Block** - conneciton to the web site is blocked, no further security processing is done, the verdict is final.
- 
**Allow** - allow connection from Web Filtering point of view, the connection will still  be checked by other security profiles/features if available - IPS/AppControl, etc and thus may be blocked later.
- 
**Monitor** - monitor (and log if logs are enabled in the security rule) but do NOT block the connection. Mostly useful to get detailed info on web sites users are visiting.
- 
**Authenticate** - access will be granted after the end user successfully authenticates to Fortigate.
- 
**Warning** - users will see a web page warning them that they are entering restricted domains, and if the user clicks on "Proceed" she will be redirected to the original web site.

Categories order of precedence: Local Category > Remote Category > Fortiguard Category.

To change the contents of **Replacement Page**, go to System → Replacement messages → click on Extended view → click on the needed template → Edit. We can change the message for actions that intercept user’s connection: Block, Warning, Authenticate.

### Category cache verification

All mappings of domains to the Category from FortiGuard Fortigate keeps in its cache, so if we want to verify what category actually a given visited domain got, we run **diag webfilter fortiguard cache dump**:

diag webfilter fortiguard cache dump
# diag webfilter fortiguard cache dump
Caution: This command is for diagnostic purposes ONLY.
The bigger the cache size is set, the more impact on
performance the command has.
Do you want to continue? (y/n)
Saving to file [/tmp/urcCache.txt]
Cache Contents:
-=-=-=-=-=-=-=-
Cache Mode:   TTL
Cache DB Ver: 234.25972
Rating            DB Ver   DOT  SLASH ORIG_FLAG T URL
00000000|00000000 234.25972     2    0 00000001 P Dhttps://104.26.8.62/
2e000000|2e000000 234.25970     1    0 00000102 E Dhttps://www.espn.com/
00000000|00000000 234.25970     1    0 00000001 P Dhttps://4.207.247.139/
34000000|34000000 234.25970     1    0 00000001 P Dhttps://client.wns.windows.com/
34000000|34000000 234.25970     1    0 00000001 P Dhttps://8.8.4.4/
34000000|34000000 234.25970     1    0 00000001 P Dhttps://dns.google/

The 1st number is the category in hex. After we translate it to decimal, we can compare with the built-in categories of the Fortigate **get webfilter categories | grep *n***:

E.g. for espn.com the hex is 2e = 46:

```
get webfilter categories | grep 46
     46 Sports
```
As we can see, most of the IP addresses do not get their rating at all, except the well-known Google DNS, that is because I have option "Rate URLs by domain and IP Address" disabled.

### Action - Authenticate

Here we are not blocking or allowing access to a Category immediately, but require additional authentication from the user to proceed and view the web site. Users can be local or remote (e.g. RADIUS/LDAP, but no FSSO/SAML meantime). Once we have user/group, we can set them in the Web Filter → Category → Authenticate. Each time user tries to access such category, she will be asked to enter user/pass, and will be allowed access for the Warning period of time (default = 5 minutes), after which the access again is blocked until the user again authenticates.

| Note | FortiOS up to 7.4.4 require proxy-mode policy for this feature to work. | 

GUI Config:

- 
Enable Category filtering, find the needed Category (here Sport), and click on Action = Authenticate.

- 
On trying to enter the website belonging to the restricted Category, the user will see:

And after clicking on "Proceed":

And after entering the user/pass combo correctly, the user will be redirected to the original website, here espn.com.

- 
In logs the above will be seen as:

# exe log filter category 3
# exe log display
899 logs found.
10 logs returned.
1: date=2025-03-05 time=15:56:37 eventtime=1741182997112761866 tz="+0200"
logid="0316013057" type="utm" subtype="webfilter" eventtype="ftgd_blk" level="warning"
vd="VMVDOM" policyid=9 poluuid="6d0debbe-1755-51ef-ca7d-d938a76591a0" policytype="policy"
sessionid=1999198506 user="yurisk1" authserver="FortiAuth" srcip=192.168.101.0
srcport=54983 srccountry="Reserved" srcintf="Peer1P1" srcintfrole="undefined"
srcuuid="55594f04-1755-51ef-321a-01bd763d3f03" dstip=3.169.71.125 dstport=443
dstcountry="United States" dstintf="VDOM-EXT" dstintfrole="undefined"
dstuuid="99376ffe-9e90-51ea-7ca2-915d04cabdb7" proto=6 httpmethod="GET" service="HTTPS"
hostname="www.espn.com" agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.
36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36 Edg/133.0.0.0" profile="WebProfile"
action="blocked" reqtype="referral" url="https://www.espn.com/service-worker.js"
direction="outgoing" msg="URL belongs to a category with warnings enabled"
ratemethod="domain" cat=46 catdesc="Sports"

- 
CLI configuration

Create a local user:

```
config user local
    edit "yurisk4"
        set type password
        set passwd-time 2025-03-05 11:46:59
        set passwd ENC +UY9yWP==
    next
end
```
Create a Firewall user group and add the user to it:

```
config user group
    edit "webauthgrp"
        set member "yurisk4"
    next
end
```
And finally, configure the Web Filter profile:

```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        config web <-- CONTENT KEYWORD BLOCKING, UNRELATED
            set bword-threshold 20
            set bword-table 1
        end
        config ftgd-wf
            unset options
            config filters
                edit 46  <-- 46 IF SPORTS CATEGORY
                    set category 46
                    set action authenticate
                    set warn-duration 2h2m <-- INCREASE WARN TIME TO 2H
                    set auth-usr-grp "webauthgrp"
                next
            end
        end
    next
end
```
### Allow User Override

If enabled, this feature allows end user, after trying to reach site that is blocked per category in the current Web Filter profile, to enter his user/pass and have Web Filter profile replaced with another profile configured beforehand. This way we can allow users in a particular user group to switch to more allowing Web Filter profile.

E.g. I create a new Web Filter profile that allows more categories than the main Web Filter, including Social Networks and News & Media named "OverrideAllowAll" (not shown here).

Then I create the main Web Filter called "WebProfile", which lists that "OverrideAllowAll" profile as a replacement after the user successfully authenticates, and in which ("WebProfile") I block Social Networks and News & Media:

In the section below, I set the user group to authenticate against (can be local and can be remote - LDAP/RADIUS) and replacement Web Filter profile for such authenticated users:

Also, note that I set override duration to 5 mins, which means after 5 minutes user will be again blocked from accessing the site with option to authenticate again.

Page with the option to override the Web Filter profile that blocks access as seen by end user:

After clicking "Override":

Pay attention to the port used by Fortigate to authenticate end user, make sure it is open:

The ports used by Fortigate for this and other features are listed here, and can be set to anything you want:
**config webfilter fortiguard**

config webfilter fortiguard
(fortiguard) # get
cache-mode          : ttl
cache-prefix-match  : enable
cache-mem-permille  : 1
ovrd-auth-port-http : 8008
ovrd-auth-port-https: 8010
ovrd-auth-port-https-flow: 8015
ovrd-auth-port-warning: 8020
ovrd-auth-https     : enable
warn-auth-https     : enable
close-ports         : disable
request-packet-size-limit: 0
embed-image         : enable

Create user group:

```
config user group
    edit "webauthgrp"
        set member "yurisk4" "localvpn1"
    next
end
```
As categories are all appear as numbers, it is easier to configure in GUI, but for completeness sake, here is the complete Web Filter profile:

```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        set ovrd-perm bannedword-override urlfilter-override fortiguard-wf-override contenttype-check-override
        config override
            set ovrd-scope ip
            set ovrd-dur 5m
            set ovrd-user-group "webauthgrp"
            set profile "OverrideAllowAll"
        end
        config ftgd-wf
            unset options
            set ovrd 83 96 98 99 26 61 86 88 90 23 30 36 37
            config filters
                edit 1
                    set category 2
                    set action warning
                next
                edit 2
                    set category 7
                    set action warning
                next
                ... CUT ...
                edit 19
                    set category 61
                    set action block
                next
                edit 20
                    set category 86
                    set action block
                next
                edit 21
                    set category 88
                    set action block
                next
                edit 22
                    set category 90
                    set action block
                next
                edit 23
                    set category 23
                    set action block
                next
                edit 24
                    set category 96
                    set action block
                next
                edit 25
                    set category 98
                    set action block
                next
                edit 26
                    set category 99
                    set action block
                next
                edit 27
                    set category 83
                    set action block
                next
                edit 28
                    set category 4
                next
                edit 29
                    set category 1
                next
                edit 30
                    set category 3
                next
                edit 31
                    set category 31
                next
                edit 32
                    set category 59
                next
                edit 33
                    set category 62
                next
                edit 34
                    set category 6
                next
                edit 46
                    set category 46
                    set action authenticate
                    set warn-duration 2h2m
                    set auth-usr-grp "webauthgrp"
                next
                edit 37
                    set category 37
                    set action block
                next
                edit 38
                    set category 30
                    set action block
                next
                edit 39
                    set category 36
                    set action block
                next
            end
        end
    next
end
```
Pay attention - we can NOT specify which category to override - ALL blocked categories are overriden by new Web Filter profile "OverrideAllowAll" and will be blocked/allowed according to the new profile.

**Verification and debug** - I didn’t find yet much info on this, except that we can clear all overrides/warning periods and thus force users to authenticate again with **dia test app ovrd 6**:

dia test application ovrd 333
1.   This menu
2.   Display stats
5.   Clear all user override entries in all vdoms
6.   Clear all warning/authentication entries in all vdoms
99.  Restart the ovrd daemon.

For debug **dia deb app urlfilter 250** will give A LOT of output, beware. And **dia deb app urlfilter -1** will given even more .

In logs of Web Filtering, all access will be with the action Block, even after authentication and actually successfully accessing the website:

2: date=2025-03-06 time=10:19:34 eventtime=1741285174244223171 tz="-0800"
logid="0316013056" type="utm" subtype="webfilter" eventtype="ftgd_blk" level="warning"
vd="root" policyid=21 poluuid="06ac0942" policytype="policy"
sessionid=10154 user="localvpn1" group="ipsecgrp" authserver="localvpn1"
srcip=192.168.17.0 srcport=53720 srccountry="Reserved" srcintf="IKEv1" srcintfrole="undefined" srcuuid="c796b842"
dstip=23.200.96.79 dstport=443 dstcountry="Ireland" dstintf="port1"
dstintfrole="undefined" dstuuid="76ab0144"
proto=6 httpmethod="GET" service="HTTPS" hostname="news.sky.com"
agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
(KH" profile="WebProfile" action="blocked" reqtype="referral"
url="https://news.sky.com/world"
referralurl="https://news.sky.com/us"
sentbyte=4107 rcvdbyte=643 direction="outgoing"
msg="URL belongs to a denied category in policy"
ratemethod="domain"
cat=36 catdesc="News and Media"

### Usage Quota

When a Category has action set to one of **Warning, Authenticate**, or **Monitor**, we can limit how much bandwidth or time the end user can consume from such a category. After a user uses up her quota (for this day) she will be blocked from the websites in such category. The usage counters reset automatically each midnight. The quota is calculated per user, and for the whole category - i.e. if a user used up Sport Category by browsing espn.com, she will be blocked for any other website which is also in this category.

| Important | Quotas will only work in **Proxy-mode** webfilters/security rules. | 

The end user will see such message of using up her quota:

In logs we will see UTM block once the quota is reached (here user blocked after using up 10 Mbytes on espn.com - Sports Category):

Note: Also check SSL Exemption list in the applied SSL Inspection Profile - if the website you are trying to enforce Usage Quota is in the list - Quota will work but ONLY for new connections. I.e. if I set 5 minutes quota on Finance Category - all websites here are by default exempted from SSL Inspection, so Fortigate has no control over already established browser session with such website. Once user exhausts her quota to say Paypal.com - she will CONTINUE browsing to the website uninterrupted, BUT if she tries to enter any new website in the Finance Category - she will be blocked on used quota. The Paypal.com will only be blocked if she opens a new browser window to it, or closes and opens browser window completely, then try to enter Paypal.com. The funny thing - logs will show "UTM blocked" on quota, but actually user will continue browsing just fine until 1 of the things above happen. So, it is not a bug but a feature.

- 
How do I know if a website is SSL exempted? 2 ways - in the browser check the SSL/TLS certificate of the website - if it is the original certificate of the website - it is exempted, but if it is a Fortigate CA certificate - then it is not (given SSL Deep Inspection is used). The other way is in Fortigate logs - the action for such website will be "passthrough":

Check SSL Exemption list in **Security Profiles → SSL/SSH Inspection**:

- 
CLI config

```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        config ftgd-wf
            unset options
            config filters
                edit 1
                    set category 2
                    set action warning
                next
                edit 31
                    set category 31
                next
                edit 46
                    set category 46
                    set action authenticate
                    set warn-duration 2h2m
                    set auth-usr-grp "webauthgrp"
                next
            end
            config quota
                edit 1
                    set category 46 <-- SPORTS CATEGORY
                    set type traffic
                    set value 10 <-- 10 MBYTES
                next
                edit 2
                    set category 31 <-- FINANCE CATEGORY
                next
            end
        end
    next
end
```
### Custom/local Categories and Web rating Override

We can override/reassign a specific website to another Fortiguard category or to the Local/Custom category. This way we can change what happens when a user tries to access it.

E.g. I will re-assign www.tripadvisor.com from Travel to the Local **custom1** category and then will set the Action to Block on it, while leaving action for the Travel category Allow.

Assign the website to the Local *custom1* category in **Security Profiles → Web Rating Overrides**:

Change the Action to Block in the actual Web Filterfor *custom1*:

Now, even though "Travel" category is allowed, the specific website is being blocked on "custom1" category:

CLI configuration:

```
config webfilter ftgd-local-rating
    edit "www.tripadvisor.com"
        set rating 140 <-- CUSTOM1 CATEGORY
    next
end
```
Set action in Fortiguard-based filter to Block:

```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        config ftgd-wf
            unset options
            config filters
            edit 0
            set category 140
                    set action block
                next
            end
```
### Remote Category filter for external threat feed

This option allows us to point Fortigate to external web server that contains as a plain text file list of URLs we want to act upon - Block/Allow/Exempt.  As I already said - the priority of categories is **Local → External/Remote → Fortiguard**.

First, we create a text file with URLs and host it on external web server. The file will look like:

https://yurisk.info/2025/02/26/fortigate-dlp-file-filtering-and-more-examples/
*.sky.com
cnn.com

1st is a complete URL, 2nd is a wildcard (no regex is supported) that will also block `sky.com`, and 3rd is an exact match.

Now, we need to create **External Connector** of type **Fortiguard Category**:

| Note | Do NOT, by mistake, chose **Domain Name** as this is for DNS FIlter, not Web Filter. | 

In which we set URL to access the external feed, optional authentication and refresh rate of data from remote server (default 5 mins):

After a few seconds, the status will change to green - Connected, also we can click on "View" to actually see the URLs read from the remote server and their (URLs) status:

Now, the category "Remote" will appear in all existing and future Web Filter Profiles (by defult in status Disabled) with the external feed we just configured, make sure to change the Action to Block/Monitor/etc.:

If a user tries to enter the website from the externel feed (and I set action to Block) - it will be blocked, even though the Fortiguard category for this website is set to Allow (categories precedence):

The block message the end user will see:

Also, the FGT blocks access to the full path URL but not to the whole website (yurisk.info):

Again, it is possible because I have Deep SSL Inspection in Web Profile, otherwise FGT would not be able to see beyong 1st slash after .info in https://yurisk.info/

This Remote feed will also be available in SSL Profile for Exemption if we need to:

## Search Engines Safe Search and Vimeo

Not much to configure here - just enable or not the option to redirect end users to the Safe Search page of the listed Search Engines. The "safety" of the search is enforced by the search providers themselves, not by the Fortigate. The config is done under Static URL section (proxy-mode only feature):

CLI:

```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        config web
            set bword-threshold 20
            set bword-table 1
            set safe-search url header <-- SAFE SEARCH CONFIG STARTS HERE
            set youtube-restrict strict
            set log-search enable
        end
    end
```
The **Vimeo** option is available in CLI only:

```
config web
    set bword-threshold 20
    set bword-table 1
    set safe-search url header
    set youtube-restrict strict
    set log-search enable
end
(web) $ get
bword-threshold     : 20
bword-table         : 1 <-- CONTENT BLOCK TABLE
urlfilter-table     : 0 <-- STATIC URL FILTER TABLE
content-header-list : 0
blocklist           : disable  <-- USE URL LIST RECEIVED FROM FSA TO BLOCK
allowlist           :
safe-search         : url header
youtube-restrict    : strict
vimeo-restrict      :    <-- VIMEO
log-search          : enable
keyword-match       :
```
Fortigate supports 2 categories for VIMEO - 7 and 134.

- 
7 - block mature content
- 
134 - block both unrated and mature content

(web) $ set vimeo-restrict 7

## Rate by both IP Address and Domain

Regularly, the FGT asks Fortiguard for categorization/rating of the **domain** the end user is trying to access only. If we enable this option, FGT will ask Fortiguard for 2 ratings - 1st of the domain (as usual), and the 2nd - of IP Address this domain resolves to. If they differ, FGT will use rating weight of each returned category - the one having higher weight will be used. I haven’t seen many admins using this option.

## Block Invalid URLs

This feature, if enabled, blocks access to websites whose SSL/TLS certificate does not contain a valid domain. Usually this feature is enabled by default.

## Content Web Filtering

Here, the Fortigate checks the contents of a web page, looking for pre-defined by us keywords or whole phrases (up to 80 characters long). We can also assign *score* to each such keyword (the default being 10), as FGT counts appearances of each keyword and sums up their scores before making final decision.

| Note | The Security Policy where such web filter is used, **has** to be in Proxy mode, not Flow. | 

Obviously, the Deep SSL Inspection profile has to be used for it to be anything effective.

Web Filter Content, complete - blocks 2 keywords "cisco/Cisco" and "palo*alto" (wildcard)

```
config webfilter content
    edit 1
        set name "Traitors"
        config entries
            edit "palo*alto" <-- KEYWORD TO LOOK FOR, WILDCARD, CASE-SENSITIVE
                set status enable
                set score 10
            next
            edit "cisco"
                set status enable
                set score 10
            next
                        edit "[Cc]isco" 
```
**(1)**
                set pattern-type regexp
                set status enable
            next
        end
    next
end
1. 
Wildcards are **case-sensitive** so just listing "cisco" would only block pages with exact word "cisco", not "Cisco" nor "CISCO". I added**regex** as pattern to account for "cisco" and "Cisco".

```
config entries
    edit "palo*alto"
        set status enable
    next
end
(palo*alto) $ get
name                : palo*alto
pattern-type        : wildcard
status              : enable
lang                : western
score               : 10
action              : block
```
Scoring:

- 
Each separte keyword entry is counted only **once** on the page. In the example above, there may be 10 words "palo alto" on the same page, but Web Filter will only count 1 word/phrase worth score of 10 points.
- 
The default score for each keyword entry is **10** . By "each keyword entry" I mean in the keywords list above - e.g. I have 2 keywords for Cisco, one matches "Cisco", 2nd "cisco" - if on the same page there both "Cisco" and "cisco", it will count as 2 words found, each adding 10 points to score, making it 20 for the page.
- 
The default score for the page to be blocked is **10** , and the default score for each individual keyword entry being also 10 means, by default, any single word from the keywords will cause blocking of the page.
- 
We can change both `bword-threshold` and`score` so that only when multiple keywords appear on the page, the page will be blocked. E.g. if we set ban-word-threshold to 20, then only pages with 2 keywords (each worth 10 points) in them will be blocked.
- 
When using **regex** they seem to count as a single appearance any number of matches. E.g. if I remove from the above keywords list the wildcard "cisco" and leave only regex "[Cc]isco" then page with "Cisco" and "cisco" will count just 1 appearance of this keyword, not 2.
- 
To force FGT to look for the whole phrases only, enclose them in double quotes.

E.g. increasing the banned words threshold per page:

```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        config web
            set bword-threshold 20
            set bword-table 1
        end
    next
end
```
Now only the page that has multiple different banned words AND when they add up to 20, will it be blocked.

The block can be seen in Logs → Security Events → Web Filtering:

The end user will see the default block page:

Log on the CLI can be seen with **exe log filter category 3**, **exe log display**:

exe log filter category 3
exe log display
25 logs found.
10 logs returned.
1: date=2025-03-05 time=11:18:31 eventtime=1741166311470738081 tz="+0200"
logid="0314012288" type="utm" subtype="webfilter" eventtype="content" level="warning"
vd="VMVDOM" policyid=9 poluuid="6d0debbe-1755-51ef-ca7d-d938a76591a0" policytype="policy"
sessionid=1880758048 user="yurisk1" authserver="FortiAuth" srcip=192.168.101.0
srcport=54401 srccountry="Reserved" srcintf="Peer1P1" srcintfrole="undefined"
srcuuid="55594f04-1755-51ef-321a-01bd763d3f03" dstip=104.20.4.235 dstport=443
dstcountry="United States" dstintf="VDOM-EXT" dstintfrole="undefined"
dstuuid="99376ffe-9e90-51ea-7ca2-915d04cabdb7" proto=6 httpmethod="GET" service="HTTPS"
hostname="pastebin.com" agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.
36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36 Edg/133.0.0.0" profile="WebProfile"
reqtype="direct" url="https://pastebin.com/DbquvfL1" sentbyte=4740 rcvdbyte=6576
direction="incoming" action="blocked" banword="[Cc]isco,cisco" msg="URL was blocked
because it contained banned word(s)."

## Proxy Options

Features that work, obviously from the name, in Proxy mode only.

- 
CLI config

```
config web-proxy profile
    edit "Auto-web-proxy-profile_qp9j5z2tc"
        config headers
            edit 1
                set name "X-GoogApps-Allowed-Domains"
                set content "github.com" <---
            next
        end
    next
end
```
```
config webfilter profile
    edit "WebProfile"
        set feature-set proxy
        set options activexfilter cookiefilter javafilter <-- BLOCK THESE
        set post-action block <-- BLOCK "POST" HTTP ACTION
        next
    end
```
- 
Block POST action - will prevent uploading files, authenticating to the web sites.
- 
`cookiefilter` - strips all cookies sent by website, will make 90% of web sites unusable today.
- 
`activexfilter` - strips all ActiveX applets from a web page, quite redundant today - no reputable site uses ActiveX any more, and all browsers have it disabled anyway.
- 
`javafilter` - strip all Java applets, the same - no one uses them anymore and browsers have it blocked by default.

## Video Filter (not part of Web Filtering)

Allows to filter Youtube content by Fortiguard categories or by Youtube Channel ID you put manually, requires Channel ID or/and Youtube API key.

## Debug and Verification

- 
First, let’s check that the Fortigate has the Web Filtering license active (for Fortiguard-based categories) with **dia deb rating** :

```
# diagnose debug rating
Locale       : english
Service      : Web-filter
Status       : Enable <-- WILL BE DISABLED IF NO WEB FILTER IS USED
                        IN SECURITY RULES
License      : Contract
Service      : Antispam
Status       : Disable
Service      : Virus Outbreak Prevention
Status       : Disable
Num. of servers : 1 <-- WITH ANYCAST, WITH UNICAST MANY SERVERS WILL
                        BE LISTED
Protocol        : https
Port            : 443
Anycast         : Enable
Default servers : Included
-=- Server List (Thu Mar 13 02:55:21 2025) -=-
IP
Weight
RTT Flags
TZ
FortiGuard-requests
Curr
Lost
Total Lost
Updated Time
173.243.141.16
-11420
89
DI
0
5710
0
0
Thu Mar 13 02:55:20 2025
```
Also, make sure under **config sys fortiguard** there is no **set webfilter-force-off enable** which turns off the FortiGuard web filtering service.

In GUI, System → Fortiguard, the valid license looks like:

- 
Make sure there are no other strange configs under sys fortiguard:

```
(fortiguard) # get
fortiguard-anycast  : enable
fortiguard-anycast-source: fortinet
protocol            : https
port                : 443  <-- WORTH CHANGING IF IT IS BEING BLOCKED to 53, 8888
load-balance-servers: 1
auto-join-forticloud: disable
update-server-location: usa
sandbox-region      :
update-ffdb         : enable
update-uwdb         : enable
update-dldb         : enable
update-extdb        : enable
update-build-proxy  : enable
vdom                :  <-- WHICH VDOM IS USED TO CONNECT TO FORTIGUARD
auto-firmware-upgrade: enable
auto-firmware-upgrade-day:
auto-firmware-upgrade-delay: 3
auto-firmware-upgrade-start-hour: 1
auto-firmware-upgrade-end-hour: 4
FDS-license-expiring-days: 15
antispam-force-off  : disable
antispam-cache      : enable
antispam-cache-ttl  : 1800
antispam-cache-mpermille: 1
antispam-license    : Contract
antispam-expiration : Fri Jan  1 2038 <-- LIC EXPIRATION DATE
antispam-timeout    : 7
outbreak-prevention-force-off: disable
outbreak-prevention-cache: enable
outbreak-prevention-cache-ttl: 300
outbreak-prevention-cache-mpermille: 1
outbreak-prevention-license: Contract
outbreak-prevention-expiration: Fri Jan  1 2038
outbreak-prevention-timeout: 7
webfilter-force-off : disable
webfilter-cache     : enable
webfilter-cache-ttl : 3600
webfilter-license   : Contract
webfilter-expiration: Fri Jan  1 2038 <-- LICENSE EXPIRATION DATE
webfilter-timeout   : 15
anycast-sdns-server-ip: 0.0.0.0
anycast-sdns-server-port: 853
sdns-options        :
source-ip           : 0.0.0.0 <-- SOURCE IP TO USE
source-ip6          : ::
proxy-server-ip     :
proxy-server-port   : 0
proxy-username      :
proxy-password      : *
ddns-server-ip      : 0.0.0.0
ddns-server-ip6     : ::
ddns-server-port    : 443
interface-select-method: auto <-- SOURCE INT OF THE REQUESTS TO FORTIGUARD,
                                CAN BE: AUTO, SD-WAN, SPECIFY
```
If you want to disable anycast and switch to unicast when having problems with FortiGuard servers reachablity, see https://yurisk.info/2021/02/21/failed-to-connect-to-fortiguard-servers-updated/

- 
To see the latest update date and versions of all downloadable databases on Fortigate, run **diag autoupdate versions** :

# diag autoupdate versions
AV Engine
---------
Version: 7.00035 signed
Contract Expiry Date: Fri Jan  1 2038
Last Updated using manual update on Thu Nov 14 00:39:00 2024
Last Update Attempt: n/a
Result: Updates Installed
Virus Definitions
---------
Version: 1.00000 signed
Contract Expiry Date: Fri Jan  1 2038
Last Updated using manual update on Mon Apr  9 19:07:00 2018
Last Update Attempt: n/a
Result: Updates Installed
--CUT--

To force Fortigate to download updates: **execute update-now**.

To debug auto updates: **diagnose debug application update -1**, **dia deb enable**, and **exe update-now**. Excerpts of debug output:

# diagnose debug application update -1
# dia deb en
# execute update-now
upd_daemon[1838]-Received update request from pid=2307
do_update[680]-Starting now UPDATE (final try)
__update_upd_comp_by_settings[495]-Disabling NIDSDB/ISDB/MUDB components.
__update_upd_comp_by_settings[499]-Disabling APPDB/IOTDB/OTDB components.
__update_upd_comp_by_settings[507]-Disabling AVEN components.
__update_upd_comp_by_settings[511]-Disabling AVDB/FLDB/MMDB components.
upd_fds_load_default_server6[1046]-Resolve and add fds usupdate.fortinet.net
ipv6 address failed.
upd_comm_connect_fds[457]-Trying FDS 173.243.141.6:443
[116] __ssl_cert_ctx_load: Added cert /etc/cert/factory/root_Fortinet_Factory.cer,
root ca Fortinet_CA, idx 0 (default)
[497] ssl_ctx_use_builtin_store: Loaded Fortinet Trusted Certs
upd_cfg_extract_sfas_version[789]-version=07004000SFAS00000-00005.00046-2502130417
pack_obj[186]-Packing obj=Protocol=3.2|Command=Update|Firmware=FGVMA6-FW-7.04-2726|
SerialNumber=FGTAWSNXXXXXX|UpdateMethod=0|AcceptDelta=1|
DataItem=07004000DBDB00100-00003
upd_status_extract_contract_info[1330]-pending registration(255)
support acct() company() industry() <-- THIS FGT IS NOT REGISTERED FOR SUPPORT

- 
Verify that the name resolving on the Fortigate works. For live queries to Fortiguard, Fortigate uses 2 hosts - **service.fortiguard.net** (without anycast) and**globalguardservice.fortinet.net** (with anycast):

execute ping service.fortiguard.net
execute ping update.fortiguard.net
execute ping guard.fortinet.net

- 
For Fortiguard Category filtering, make sure the websites are categorized correctly by looking at the cache with **diag webfilter fortiguard cache dump** , see Category cache verification for example output.

If the cache is not up-to-date, use **dia test app urlfilter 2** to clear the cache (no downtime).

To see the whole list of Categories with their respective numbers, run **get webfilter categories**

```
FGT-Perimeter # get webfilter categories
  g01 Potentially Liable:
      1 Drug Abuse
      3 Hacking
      4 Illegal or Unethical
      5 Discrimination
--CUT--
```
- 
Logs - Web Filter logs are under Events → Security Events → Web Filter menu or on the CLI it is **exe log filter category 3** and**exe log display** .  See example at [logs_cli]
- 
Regular **dia deb flow** debug is useful as well. E.g. for an internal host 192.168.17.0:

diagnose debug reset
diagnose debug flow trace stop
diagnose debug flow filter clear
diagnose debug flow filter addr 192.168.17.0
diagnose debug flow filter port 443
diagnose debug flow show function-name enable
diagnose debug console timestamp enable
diagnose debug flow trace start 1000
dia deb enable

- 
Finally, the most verbose Web Filtering debug is **dia deb app urlfilter -1** ,
the example output:

2025-03-13 04:47:38 0(2190) Stop quota EP localvpn1, cat 52
2025-03-13 04:47:38 0(2190) action=10(ftgd-block) wf-act=3(BLOCK)
user="localvpn1" src=192.168.17.0 sport=56505 dst=4.207.247.137
dport=443 service="https" cat=52 url_cat=52 ip_cat=0
hostname="client.wns.windows.com" url="/"