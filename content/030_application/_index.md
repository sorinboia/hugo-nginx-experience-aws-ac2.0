+++
title = "Deploy The Arcadia Crypto application"
menuTitle = "Arcadia deployment"
date = 2020-07-08T14:37:59+03:00
weight = 30
chapter = true
pre = "<b>3. </b>"
+++

We will deploy our application in the Kubernetes environment.  
As stated before these are the 6 microservices which we will deploy.
- **Frontend** - serves the non dynamic content for like html, js, css and images
- **Login** - in in charge of dealing with anything related to the login user functionality
- **Users** - all user data interaction is done through this microservice only
- **Stocks** - connects to external resources to get the latest crypto data and serves it to the application clients
- **Stocks Transaction** - Deal with all related to buying or selling crypto currencies. It interact with other microservices like Users and Stocks
- **Database** - Database were all information is stored  

![](/images/Slide1.JPG)
