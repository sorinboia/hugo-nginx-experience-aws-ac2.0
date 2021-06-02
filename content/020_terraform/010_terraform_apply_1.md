+++
title = "Terraform Apply"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. All configuration and management of the infrastructure will be done from the Jumphost.  
   SSH into the Jumphost.

2. Watch the terraform deployment status.

```
tail -f startup/startup.log
```

When the the `ALL IS DONE` message will appear in the logs you can continue. 