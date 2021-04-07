+++
title = "Cloud Formation"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++

1. Click the bellow button and deploy the template:

[![Launch Stack](/images/cfls.svg)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://sorinnginx.s3.eu-central-1.amazonaws.com/nginx-cft.yaml)

2. Click **Next** and enter a unique "Stack name"
  
3. Click **Next** 2 times accepting all the defaults, but make sure the following is selected on the last screen:


![CFN IAM Ack](/images/iam-ack.png)


4. Click **Create stack**.

Wait until the stack Status is **CREATE_COMPLETE**.
