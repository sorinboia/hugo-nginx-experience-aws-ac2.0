+++
title = "Security"
date = 2020-07-08T14:37:59+03:00
weight = 60
chapter = true
pre = "<b>6. </b>"
+++

In our final part of the workshop, we will implement a per-pod Web Application Firewall.  
The Nginx WAF will allow to improve the application security posture, especially against [OWASP Top 10 attacks](https://owasp.org/www-project-top-ten/).  

In our scenario, since we decided our Nginx WAF to be enabled on a per-pod basis, we will be able to protect all the traffic coming into the pod regardless of where it is originating from (external or internal to the Kubernetes cluster).  

We'll be able to bring security closer to the application and the development cycle and integrate it into CI/CD pipelines.  
This will allow to minimize false positives, since the WAF policy becomes a part of the application and is always tested as such.