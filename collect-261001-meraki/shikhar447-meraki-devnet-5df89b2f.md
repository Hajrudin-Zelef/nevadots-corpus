This repository contains a Python client for the Meraki API, which can be used to fetch lists of organizations, networks within an organization, and devices within a network. The client also includes a TTL caching mechanism to store the data fetched in the first execution to speed up future executions.
To use this script, you will need:
A Cisco DevNet account (you can sign up for free at https://developer.cisco.com/) Access to the always-on IOS-XR sandbox (you can find more information on how to access it at https://developer.cisco.com/meraki/api-v1/)
Before using the Meraki API client, you will need to obtain an API key from the Meraki Dashboard Sandbox. Once you have your API key, you can either set it as an environment variable or pass it as a parameter when initializing the client.
To install the necessary dependencies, you can use pip:
pip install -r requirements.txt
export API_URL=<MERAKI_API_URL>
export MERAKI_X_AUTH_TOKEN=<MERAKI_X_AUTH_TOKEN>
python get_meraki.py
This script is licensed under the BSD 3-Clause License. See the LICENSE file for more information.