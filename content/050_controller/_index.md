+++
title = "Nginx Controller"
date = 2020-07-08T14:37:59+03:00
weight = 50
chapter = true
pre = "<b>5. </b>"
+++

We have finished the first part of the publishing our application, now we want to publish our APIs to be used by third party organizations.
We will acomplish this using two components:

- Nginx Controller which will be used as an API Management
- Nginx Container will be the API Microgateway which will reside within the Kubernetes environment