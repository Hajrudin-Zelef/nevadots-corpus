I need to rename an existing Fortigate 100D VDOM. I cannot find the command unless it's really counter-intuitive. Furthermore, if this is not possible, can I copy the existing VDOM to a new one with the correct name?
- 
        Did the answer help you? If so, you should accept the answer so that the question doesn't keep popping up forever, looking for an answer.Ron Maupin– Ron Maupin ♦2017-07-21 13:01:13 +00:00Commented Jul 21, 2017 at 13:01
1 Answer 1
Be forewarned, you will probably need to reboot the Fortigate for the changes to take place.
- Backup your entire config (not just the vdom)
- Open the config in a text editor
- Do a Search and replace of the old vdom name with the new name.
- Save the config under a new name, so that you have a backup of the old config.
- Restore the new config.
- Your vdoms should now be renamed.