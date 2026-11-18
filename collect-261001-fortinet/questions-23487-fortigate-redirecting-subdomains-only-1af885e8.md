I'm trying to figure out a way to have our Fortigate 60C redirect certain subdomains but not block complete access to the domain.
E.G. I want to block update.microsoft.com but I don't want microsoft.com or office.microsoft.com to be blocked.
My current settings are blocking the complete domain. I do find this strange though as I specified a hostname.
config system dns-database 
edit "test" 
config dns-entry 
edit 2 
set hostname "update" 
set ip 1.1.1.1 
next
end
set domain "microsoft.com"
My goals is to prevent certain software from accessing certain urls to prevent devices from downloading updates or phoning home.
1.1.1.1 is set as a loopback.
Blocking access on devices itself is not an option.