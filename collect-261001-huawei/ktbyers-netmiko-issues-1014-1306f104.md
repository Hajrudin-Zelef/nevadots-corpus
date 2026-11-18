- 
                Notifications
    You must be signed in to change notification settings
- Fork 1.4k
Description
Hi, folks!
I'm trying to save current config on Huawei Quidway S5710(VRP5) and strugglin from error:
Script:
from netmiko import ConnectHandler
host = {'ip': '10.0.0.1',
'username': 'user',
'password': 'password',
'device_type': 'huawei', #  ssh
'auth_timeout': 10,
'session_timeout': 30,
'timeout': 60
}
commands = [' acl 2002',
' rule  permit source 10.0.0.0 0.0.0.255'
]
connect = ConnectHandler(**host)
output = connect.send_config_set(commands)
print(output)
output = connect.save_config(confirm=True,confirm_response=u'y')
print(output)
Output:
system-view
Enter system view, return user view with Ctrl+Z.
[RR10-2] acl 2002
[RR10-2-acl-basic-2002] rule  permit source 10.184.254.0 0.0.0.255
Error: The rule already exists.
[RR10-2-acl-basic-2002]return
The current configuration will be written to the device.
Are you sure to continue?[Y/N]Error: Please choose 'YES' or 'NO' first before pressing 'Enter'. [Y/N]:
Traceback (most recent call last):
File "C:\Users\user\Documents\work\test.py", line 48, in 
connect.send_command(u'y')
File "C:\Users\user\Documents\WPy-3670\python-3.6.7.amd64\lib\site-packages\netmiko\base_connection.py", line 1188, in send_command
search_pattern))
OSError: Search pattern never detected in send_command_expect: Error:\ Please\ choose\ 'YES'\ or\ 'NO'\ first\ before\ pressing\ 'Enter'.\ [Y/N]:
can anybody help me? what I do wrong?
Activity
@JurgenOS How did you solve this problem?