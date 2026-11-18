**Description**


This article describes that if the firewall policy is set to flow-mode inspection, it will not be possible to apply a web filter or an antivirus profile to it. This is because both Antivirus and web filter by default will be created with the Feature-set in Proxy. 


**Scope**


FortiGate.

**Solution**


The profile will not be visible in the drop-down menu in those cases.



Go to **Security Profiles -> Web Filter**, select the profile, and next to 'Feature set', select 'Flow-based' and then select 'OK'.

**From CLI:**


**config webfilter profile**

    edit "test"

        set feature-set flow

       next

end


Or go to '**Security Profiles ' -> 'Anti-Virus',** select the profile and choose **'Flow-Based'** from the Feature set.


**From CLI:**


**config antivirus profile**

    edit "test"

        set feature-set flow

    next

end


Go back to the **' Policy** **& Object ' -> ' Firewall policy '** and edit the policy to select the profile 'test' web filter and Anti-virus from the above examples.



**From CLI:**


**config firewall policy**

    edit 3

        set av-profile "test"

        set webfilter-profile "test"

    next

end