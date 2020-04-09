---
Title: Turn of middle click page loading in Firefox
Date: 2010-06-23 13:09:00
---



In linux, the middle mouse button by default pastes the contents of the
clipboard into something. If I had a penny for every time I've miss
middle-clicked a link and tried to open a url like "for \$file in
\`ls\`"...

To disable this "feature", go to about:config in firefox then change


> middlemouse.contentLoadURL -\> false


This will disable middle-click url loading.
