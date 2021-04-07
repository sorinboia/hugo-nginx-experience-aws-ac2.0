+++
title = "Deploy your application with NGINX Unit Application Server"
menuTitle = "Nginx Unit"
date = 2020-07-08T14:37:59+03:00
weight = 30
chapter = true
pre = "<b>3. </b>"
+++

We will deploy our application in the Kubernetes environment.  
As stated before these are the 4 microservices which we will deploy.
- **Main** - provides access to the web GUI of the application for use by browsers
- **Backend** - is a supporting microservice and provides support for the customer facing services only
- **App2** - provides money transfer API based functionalities for both the Web app and third party consumer applications
- **App3** - provides referral API based functionalities for both the Web app and third party consumer applications  
