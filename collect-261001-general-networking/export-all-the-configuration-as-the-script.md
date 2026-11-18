Good day…

Dear All…

Is there anyway we can export the back-up file in the script type instead of the .bak files?

with this, we can configure new replacement unit with just copy and paste the script will do.

thanks.

             
            
           
          
            
            
              This is one of those few things that are pretty decently covered in the manual.

```
:export file=mybackup
```

And you’ll have a mybackup.rsc in the /file area.

             
            
           
          
            
            
              
For any version more recent than 5.12 use “export compact”.

```
/
export compact file=mybackup
```

That should give you just the settings that have been changed from the default values, which makes things easier if you need to do any editing.