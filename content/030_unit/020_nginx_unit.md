+++
title = "Nginx Unit"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++

All of our pods are created based on the Nginx Unit application server.
NGINX Unit is a dynamic application server, capable of running beside NGINX Plus and NGINX Open Source or standalone. Unit supports a RESTful JSON API, deploys configuration changes without service disruptions, and runs apps built with multiple languages and frameworks. Designed from scratch around the needs of your distributed applications, it lays the foundation for your service mesh.

The main features are:  
##### Polyglotism
Configure all your applications with one streamlined interface:
- Uniform support for Go, Node.js, Perl, PHP, Python, and Ruby
- Applications written in different languages run on the same server
- Different versions of a language run side by side (PHP 5 and PHP 7, Python 2.7 and Python 3)

##### Programmability
Adjust to your applications’ needs on-the-fly:
- Comprehensive RESTful API simplifies configuration
- JSON syntax provides visibility and transparency
- In‑memory updates reduce service disruptions
- Zero-downtime deployments facilitate seamless updates

##### Service Mesh
Use NGINX Unit as the foundation for your service mesh:
- Integrated network stack for fast service-to-service communication
- Offload network configuration from application code to NGINX Unit
- Built-in SSL/TLS support

More information can be found [here](https://www.nginx.com/products/nginx-unit/#resources).  
![](/images/4env.JPG)