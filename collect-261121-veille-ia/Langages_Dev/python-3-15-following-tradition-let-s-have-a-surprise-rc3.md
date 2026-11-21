---
id: collect-261121-veille-ia/langages-dev/python-3-15-following-tradition-let-s-have-a-surprise-rc3
title: "Python 3.15: following tradition, let's have a surprise rc3!"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["python", "final", "surprise", "following", "tradition", "test", "classifier", "release"]
source: https://discuss.python.org/t/python-3-15-following-tradition-lets-have-a-surprise-rc3/109313
source_anchor: ""
source_lines: [1, 74]
section: "Langages_Dev"
---
Following recent tradition, I’m going to add a surprise third release candidate for Python 3.15.0!

We got some last-minute lazy-import release blockers, and it makes sense to include them in 3.15.0 final. It also makes sense to give a bit of time to test them properly, so today’s planned 3.15.0 final is postponed, and I’m going to release a 3.15.0rc3 tomorrow, assuming everything is in place.

And then 3.15.0 final will follow about a week later. Exact date to be announced.

- 
Everyone: once the rc3 is out, please test!
- 
Library maintainers: join the cool testers’ club !
- 
Core team: very little will be merged into the `3.15` branch between now and 3.15.0 final. Docs improvements always welcome.

Thanks, we’re nearly there!

             
              
                  
                  
                38 Likes
              

            
           
          
            
            
              Quick question: in Help test Python 3.15! · Hugo van Kemenade, you recommend adding the 3.15 trove classifier. I thought I should add such a classifier only once 3.X.0 final is released. Should I rather add it as soon as a 3.X.0 alpha (beta?) is out?

             
              
                  
                  
                1 Like
              

            
           
          
            
            
              You can add it whenever you want to signal “I’m testing this version, believe my code supports it, and have made wheels available for you”.

22.2% of the top 360 packages already have a 3.15 classifier, and one (coverage) already has the 3.16 classifier!

             
              
                  
                  
                3 Likes
              

            
           
          
            
            
              Surprise 

Everyone *please test*, **3.15.0 final** is planned for **2026-10-09**.

             
              
                  
                  
                9 Likes
              

            
           
          
            
            
              A post was split to a new topic: How do I install Python 3.15rc3?
