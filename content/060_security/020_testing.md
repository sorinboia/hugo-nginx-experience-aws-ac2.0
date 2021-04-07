+++
title = "Testing the Waf"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++

1. Browse again to the Arcadia web app and verify that it is still working.  

2. Let's simulate a Cross Site Scripting (XSS) attack, and make sure it's blocked:  

`https://<INGRESS-EXTERNAL-IP>/trading/index.php?a=%3Cscript%3Ealert(%27xss%27)%3C/script%3E`

Each of the blocked requests will generate a support ID, save it for later.  

3. Browse to the ELK as before and click the "Discover" button:  

![](/images/kibana1.JPG)  

  
  
Here, you'll see all the request logs, allowed and blocked, sent by the Nginx WAF to ELK.  

Let's look for the reason why our attack requests were blocked.  


4. Add a filter with the support ID you have received as seen bellow:
  
![](/images/kibana2.JPG)  

In the right side of the panel, you can see the full request log and the reason why it was blocked.  

5. Continue and explore the visualization capabilities of Kibana and log information from Nginx WAF by looking into the next two sections bellow the "Discover" button (Visualize and Dashboard -> Overview).  

  

![](/images/2env.jpg)