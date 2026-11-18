A PHP class that provides access to Ubiquiti's UniFi Network Application API.
This class is used by our API Browser tool, which can be found here.
The package can be installed manually or by using composer/packagist for easy inclusion in your projects. See the installation instructions below for more details.
- Easy to use: clear docs, comprehensive method coverage, and helpful examples.
- Broad coverage: exposes many UniFi endpoints not (yet) available in the official APIs.
- Composer-friendly: installable via Composer and works with modern PHP projects.
- Lightweight and dependency-free: no external libraries required; uses cURL.
- Secure: communicates over TLS and supports optional SSL certificate validation.
- Flexible and extensible: includes custom_api_request() for calling any API endpoint.
- Robust error handling: throws named Exceptions for precise try/catch handling.
- Actively maintained: regular updates and compatibility with recent UniFi versions.
| Software | Versions | 
|---|---|
| UniFi Network Application/controller | 5.x, 6.x, 7.x, 8.x, 9.x, 10.x (10.5.62 is confirmed) | 
| UniFi OS | 3.x, 4.x, 5.x (5.1.26 is confirmed) | 
- a server or desktop with:
  - PHP 7.4.0 or higher (use version 1.1.83 for PHP 7.3.x and lower)
  - PHP cURL (php-curl ) module enabled
  - PHP JSON (php-json ) module enabled (always available on PHP 8.0 and later, but it can be
disabled at compile time on PHP 7.4)
  - direct network connectivity between this server/desktop and the host and port where the UniFi Network Application is running (usually TCP port 8443, port 11443 for UniFi OS Server, or port 443 for UniFi OS consoles)
- authentication — you need one of the following:
  - an admin account with local access permissions as explained here: https://artofwifi.net/blog/use-local-admin-account-unifi-api-captive-portal Do not use UniFi Cloud accounts and do not enable MFA/2FA for these accounts. (see Option 2: Username/Password below)
  - or an API key generated in your UniFi OS console or UniFi OS Server (see Option 1: API Key below)
  - or a Site Manager API key for routing requests through the Ubiquiti cloud proxy (see Option 3: Site Manager Proxy below)
Starting from version 1.1.47, this API client also supports UniFi OS-based controllers. These applications/devices/services have been verified to work:
- UniFi OS Server, announcement here
- UniFi Dream Router (UDR)
- UniFi Dream Machine (UDM)
- UniFi Dream Machine Pro (UDM PRO)
- UniFi Cloud Key Gen2 (UCK G2), firmware version 2.0.24 or higher
- UniFi Cloud Key Gen2 Plus (UCK G2 Plus), firmware version 2.0.24 or higher
- UniFi Express (UX)
- UniFi Dream Wall (UDW)
- UniFi Cloud Gateway Ultra (UCG-Ultra)
- UniFi CloudKey Enterprise (CK-Enterprise)
- UniFi Enterprise Fortress Gateway (EFG)
- Official UniFi Hosting, details here
- HostiFi UniFi Cloud Hosting, details here
The API Client automatically detects UniFi OS consoles/servers and adjusts URLs and several functions/methods accordingly.
UniFi OS-based consoles require you to connect using port 443, whereas port 8443 is used for the self-hosted/software-based controllers. When connecting to UniFi OS Server, you are required to use port 11443.
When connecting to a UniFi OS-based gateway through the WAN interface, you need to create a specific firewall rule to
allow this. See this blog post on the Art of WiFi website for detailed instructions when using the "Classic"
firewall:
https://artofwifi.net/blog/how-to-access-the-unifi-controller-by-wan-ip-or-hostname-on-a-udm-pro
See this blog post when using the Zone-Based firewall (ZBF):
https://artofwifi.net/blog/how-to-access-the-unifi-controller-by-wan-ip-or-hostname-on-a-udm-pro-using-zbf
When upgrading from a version before 2.0.0, please:
- change your code to use the new Exceptions that are thrown by the API Client class
- test the client with your code for any breaking changes
- make sure you are using Composer to install the API Client because the code is no longer held within a single file
- see the note here regarding the single file version (1.x.x) of the API client
The preferred installation method is through Composer. Follow these installation instructions if you don't have Composer installed already.
Once Composer is installed, execute this command from the shell in your project directory:
composer require art-of-wifi/unifi-api-client
Or manually add the package to your composer.json file:
{
    "require": {
        "art-of-wifi/unifi-api-client": "^2.0"
    }
}
Finally, be sure to include the composer autoloader in your code if your framework doesn't already do this for you:
/**
 * load the class using the composer autoloader
 */
require_once 'vendor/autoload.php';
The API client supports three authentication methods. Choose the one that fits your setup.
API key authentication is stateless — no login/logout flow is needed. This is the simplest way to get started with UniFi OS-based controllers.
require_once 'vendor/autoload.php';
$unifi_connection = new UniFi_API\Client('', '', 'https://unifi:443', 'default');
$unifi_connection->set_api_key('your-api-key-here');
$results = $unifi_connection->list_alarms(); // no login() needed
How to generate an API key:
- Open your UniFi OS console in a browser
- Navigate to Integrations in the sidebar menu
- Click Create New API Key
- Copy the generated key — it will not be shown again
The API key inherits the permissions from the admin user that created it.
Key points:
- API keys are only available on UniFi OS-based consoles (UDM, UDR, UCG, UX, UDW, UCG-Ultra, UniFi OS Server, etc.)
- No login() orlogout() calls are needed (calling them is harmless and will be ignored)
- The client automatically configures itself for UniFi OS when an API key is set
The traditional authentication method that works with both self-hosted controllers and UniFi OS consoles.
require_once 'vendor/autoload.php';
$unifi_connection = new UniFi_API\Client(
    $controller_user,
    $controller_password,
    $controller_url,
    $site_id
);
$login   = $unifi_connection->login();
$results = $unifi_connection->list_alarms();
Requirements for username/password authentication:
- You must use a local admin account with local access permissions as explained here: https://artofwifi.net/blog/use-local-admin-account-unifi-api-captive-portal
- Do not use UniFi Cloud accounts
- Do not enable MFA/2FA on accounts used with this client
If your console is managed through unifi.ui.com and you don't have a direct network path to it, you can route API requests through the Ubiquiti Site Manager cloud proxy. This uses the Site Manager API connector to reach the console via UI.com's cloud infrastructure.
require_once 'vendor/autoload.php';
$client = UniFi_API\Client::connect_via_site_manager(
    '245A4CA234150000000005F23204000000000638FE970000000061156371:48913759', // console ID
    'your-site-manager-api-key',                                              // Site Manager API key
    'default'                                                                  // site (optional)
);
// No login() needed — proxy mode is stateless
$stats = $client->stat_daily_site();
Finding the console ID:
The console ID (host ID) is visible in the URL when managing a console via unifi.ui.com:
https://unifi.ui.com/consoles/{console_id}/network/default/dashboard
Site Manager API key: Generate a Site Manager API key at https://unifi.ui.com under your account settings. This is not the same as a local controller API key generated in the UniFi OS console.
Requirements:
- Console firmware version must be >= 5.0.3
- Console must be online and connected to UI.com
- For non-organization API keys: limited to the API key owner's consoles only
- For organization API keys: can access any console within the organization
You can also enable/disable proxy mode on an existing client instance:
$client = new UniFi_API\Client('', '', 'https://127.0.0.1', 'default');
$client->enable_site_manager_proxy($console_id, $site_manager_api_key);
// ... make API calls through the proxy ...
$client->disable_site_manager_proxy();
// Client must be re-configured (login, set_api_key, etc.) before making direct calls
SSL verification:
In proxy mode, all requests go to api.ui.com which has a valid public CA certificate. SSL peer and host
verification is automatically enforced.
Performance note: Proxied requests add ~800ms of latency compared to direct access due to the cloud hop. Use direct access when available. The proxy is best suited for remote/headless deployments where a direct network path does not exist.
Rate limits:
The Site Manager API enforces rate limits (10,000 requests/minute for v1 stable). When exceeded,
the API returns HTTP 429 with a Retry-After header.
| Scenario | Method | 
|---|---|
| Controller is part of a UniFi Fabric | API key (required) | 
| UniFi OS console (UDM, UDR, UCG, etc.) | API key (recommended) or username/password | 
| UniFi OS Server | API key (recommended) or username/password | 
| Self-hosted Network Application (non-UniFi OS Server) | Username/password (API keys are not supported) | 
| Remote console via UI.com (no direct network path) | Site Manager proxy | 
Important: When your controller is a member of a UniFi Fabric, username/password authentication is not available. You must use API key authentication. UniFi Fabric uses centralized identity management which does not support local login sessions through the API.
If you have existing code using username/password authentication and want to switch to API keys, the changes are minimal:
Before (username/password):
$unifi_connection = new UniFi_API\Client($user, $password, $url, $site_id);
$unifi_connection->login();
$results = $unifi_connection->list_alarms();
$unifi_connection->logout();
After (API key):
$unifi_connection = new UniFi_API\Client('', '', $url, $site_id);
$unifi_connection->set_api_key($api_key);
$results = $unifi_connection->list_alarms();
What changes:
- Pass empty strings for the $user and$password constructor parameters
- Call set_api_key() instead oflogin()
- Remove logout() calls (or leave them — they will be silently ignored)
- In your exception handling, LoginFailedException andLoginRequiredException will no longer occur;
you can keep or remove those catch blocks as you prefer
- 
In the above example, $site_id is the short site "name" (usually 8 characters long) that is visible in the URL when
managing the site in the UniFi Network Controller. For example, with this URL:https://<controller IP address or FQDN>:8443/manage/site/jl3z2shm/dashboardjl3z2shm is the short site "name" and the value to assign to $site_id.
- 
The 6th optional parameter that is passed to the constructor in the above example ( true ), enables validation of
the controller's SSL certificate, which is otherwise disabled by default. It is highly recommended to enable
this feature in production environments where you have a valid SSL cert installed on the UniFi Controller that is
associated with the FQDN in thecontroller_url parameter. This option was added with API client version 1.1.16.
- 
Using an administrator account ( $controller_user in the above example) with read-only permissions can limit
visibility on certain collection/object properties. See this
issue and this
issue for an example where the WPA2 password isn't
visible for read-only administrator accounts.
More code examples are available in the examples/ directory.
The API Client class throws Exceptions for various error conditions instead of using PHP's trigger_error()
function. This allows for more granular error handling in your application code.
You can also choose to catch the UniFi_API\Exceptions\UnifiApiException Exception to catch all Exceptions that
might be thrown by the API Client class.
Here is an example of how to catch each of the Exceptions individually:
<?php
/**
 * PHP API usage example with Exception handling
 */
use UniFi_API\Exceptions\ConsoleOfflineException;
use UniFi_API\Exceptions\CurlExtensionNotLoadedException;
use UniFi_API\Exceptions\CurlGeneralErrorException;
use UniFi_API\Exceptions\CurlTimeoutException;
use UniFi_API\Exceptions\InvalidBaseUrlException;
use UniFi_API\Exceptions\InvalidSiteNameException;
use UniFi_API\Exceptions\JsonDecodeException;
use UniFi_API\Exceptions\LoginFailedException;
use UniFi_API\Exceptions\LoginRequiredException;
/**
 * load the class using the composer autoloader
 */
require_once 'vendor/autoload.php';
/**
 * include the config file (place your credentials etc. there if not already present)
 */
require_once 'config.php';
/**
 * initialize the UniFi API connection class, log in to the controller and request the alarms collection
 * (this example assumes you have already assigned the correct values to the variables in config.php)
 */
try {
    $unifi_connection = new UniFi_API\Client($controller_user, $controller_password, $controller_url, $site_id, $controller_version, true);
    $login            = $unifi_connection->login();
    $results          = $unifi_connection->list_alarms(); // returns a PHP array containing alarm objects
} catch (CurlExtensionNotLoadedException $e) {
    echo 'CurlExtensionNotLoadedException: ' . $e->getMessage(). PHP_EOL;
} catch (InvalidBaseUrlException $e) {
    echo 'InvalidBaseUrlException: ' . $e->getMessage(). PHP_EOL;
} catch (InvalidSiteNameException $e) {
    echo 'InvalidSiteNameException: ' . $e->getMessage(). PHP_EOL;
} catch (JsonDecodeException $e) {
    echo 'JsonDecodeException: ' . $e->getMessage(). PHP_EOL;
} catch (LoginRequiredException $e) {
    echo 'LoginRequiredException: ' . $e->getMessage(). PHP_EOL;
} catch (ConsoleOfflineException $e) {
    echo 'ConsoleOfflineException: ' . $e->getMessage(). PHP_EOL;
} catch (CurlGeneralErrorException $e) {
    echo 'CurlGeneralErrorException: ' . $e->getMessage(). PHP_EOL;
} catch (CurlTimeoutException $e) {
    echo 'CurlTimeoutException: ' . $e->getMessage(). PHP_EOL;
} catch (LoginFailedException $e) {
    echo 'LoginFailedException: ' . $e->getMessage(). PHP_EOL;
} catch (Exception $e) {
    /** catch any other Exceptions that might be thrown */
    echo 'General Exception: ' . $e->getMessage(). PHP_EOL;
}
Although the PHP DocBlocks for most public methods/functions contain @throws Exception, it is recommended to catch
specific Exceptions that can be thrown by the API Client class to provide more detailed error messages to your
application code.
In most cases, the class will let Exceptions bubble up to the calling code, but in some cases it will catch them and throw a new Exception with a more specific message.
The list_alarms.php example in the examples/ directory is a good starting point to see how you can implement
Exception handling.
The API Client class currently supports a large and growing number of functions/methods to access the UniFi Controller API. Please refer to the comments/PHP DocBlocks in the source code for more details on each of the functions/methods, their purpose, and their respective parameters.
If you are using an advanced IDE such as PHPStorm or VS Code, you can use its code completion and other features to explore the available functions/methods thanks to the extensive PHP DocBlocks throughout the code.
For a quick overview of the available functions/methods, you can also check the API Reference here:
API Reference
There is still work to be done to add functionality and further improve the usability of this API Client class, so all suggestions/comments are welcome. Please use the GitHub Issues section or the Ubiquiti Community forums (https://community.ui.com/questions/PHP-client-class-to-access-the-UniFi-controller-API-updates-and-discussion-part-2/a793904e-6023-4a7f-bcae-340db2a03fc1) to share your suggestions and questions.
When encountering issues with the UniFi API using other libraries, cURL or Postman, please do not open an Issue. Such issues will be closed immediately. Please use the Discussions section instead.
With versions 1.x.x of the API client, the entire client was contained within a single file which can be useful in specific cases. This has changed with version 2.0.0 where the code is now split across multiple files and inclusion in your project is managed using composer.
If you are looking for the version 1.x.x code, you can tell composer to install that version by using the following
syntax in your composer.json file:
{
    "require": {
        "art-of-wifi/unifi-api-client": "^1.1"
    }
}
Alternatively, you can download the latest 1.x.x code from the releases page.
Whenever necessary, we will make sure to update the version_1 branch with the latest 1.x.x code.
This API Client class is based on the initial work by the following developers:
- domwo: https://community.ui.com/questions/little-php-class-for-unifi-api/933d3fb3-b401-4499-993a-f9af079a4a3a
- fbagnol: https://github.com/fbagnol/class.unifi.php
and the API as published by Ubiquiti:
A big thanks to all the contributors who have helped with this project!
If you would like to contribute to this project, please open an issue and include your suggestions or code there or else create a pull request.
Art of WiFi develops software and tools that enhance the capabilities of UniFi networks. From captive portals and reporting solutions to device search utilities, our goal is to make UniFi deployments more powerful and easier to manage.
If you're looking for a specific solution or just want to see what else we offer, feel free to explore our website:
Many of the functions in this API client class are not officially supported by Ubiquiti and as such, may not be supported in future versions of the UniFi Controller API.