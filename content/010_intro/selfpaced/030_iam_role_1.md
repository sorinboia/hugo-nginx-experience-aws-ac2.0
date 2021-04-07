+++
title = "IAM Role"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++

1. Follow [this deep link to find your Cloud9 EC2 instance](https://console.aws.amazon.com/ec2/v2/home?#Instances:tag:Name=aws-cloud9-ideNGINX.*;sort=desc:launchTime)


2. Select the instance, then choose **Actions / Instance Settings / Attach/Replace IAM Role**

![c9instancerole](/images/c9instancerole.png)

3. Choose **eksworkshop-admin** from the **IAM Role** drop down, and select **Apply**

![c9attachrole](/images/c9attachrole.png)
