+++
title = "Intro to the workshop"
date = 2020-07-08T14:11:17+03:00
weight = 10
chapter = true
pre = "<b>1. </b>"
+++


Intro to the workshop

This workshop will provide guidelines on how to deploy an application from scratch in Amazon Elastic Kubernetes Service environment while protecting and enhancing the application availability and usability with Nginx solutions.

For this workshop we are going to use the "Arcadia Financial" application.
The application is built with 4 different microservices that are deployed in the Kubernetes environment.
- Main - provides access to the web GUI of the application for use by browsers
- Backend - is a supporting microservice and provides support for the customer facing services only
- App2 - provides money transfer API based functionalities for both the Web app and third party consumer applications
- App3 - provides referral API based functionalities for both the Web app and third party consumer applications



By the end of the workshop the "Arcadia Financial" will be fully deployed and protected as described in the bellow diagram.

![](/images/2env.jpg)