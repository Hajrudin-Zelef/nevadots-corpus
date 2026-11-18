Use script to transfer configuration.
/export file=config.rsc
Transfer script to PC.
Fix script by removing mac-address options from interfaces at begining of script.
Transfer to other Mikrotik
And apply script.
/system reset-configuration no-defaults=yes run-after-reset=config.rsc
PS. For successful applying configuration new router must have enabled all packages which configuration exported.
Export file can be edited to cover some special cases (I know, user-manager configuration don't cleared by /system reset-configuration)