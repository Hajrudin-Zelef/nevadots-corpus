We have Catalyst 4500R+E with Sup7L-E (IOS XE 3.4SG). It was bundle with LAN Base License Installed.
#show license summary
Index 0   Feature: entservices
          Period left: 8 weeks 4 days
          License Type: Evaluation
          License State: Active, Not in Use, EULA not accepted
          License Count: Non-Counted
          License Priority: None
Index 1   Feature: ipbase
          Period left: 0 seconds
Index 2   Feature: lanbase
          Period left: Life time
          License Type: Permanent
          License State: Active, In Use
          License Count: Non-Counted
          License Priority: Medium
Index 3   Feature: internal_service
          Period left: 0 seconds
Also I read Licensing Deployment Guide and there was very interesting infoirmation:
  User will be notified of the impending license expiry before it expires. After the evaluation license expires, the system continues to run at that license level. However, syslog messages indicating the expired license are continuously displayed on the console, at an interval of one every five minutes.
  If for any reason the system reloads and the "license boot level ipbase | entservices" IOS CLI is saved as part of startup-configuration, the system overrides the base license, and continues to boot in the license level specified in the IOS CLI, even though it is expired. This behavior might be changed in future when licenses will be enforced.
Does it mean that if I save this line to configuration license boot level ipbase | entservices, I will have working perpetual Enterprise License but with some warnings in syslog?