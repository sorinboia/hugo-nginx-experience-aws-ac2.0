+++
title = "Service Mesh Visibility"
date = 2020-07-08T14:37:59+03:00
weight = 40
+++


In order to be able and trace and monitor all of our application calls we will need to expose the Grafana and Zipkin servers 


1. Expose Grapha and Zipkin

```
kubectl get svc -n nginx-mesh grafana -oyaml | sed 's/ClusterIP/LoadBalancer/g' | kubectl apply -f -
kubectl get svc -n nginx-mesh zipkin -oyaml | sed 's/ClusterIP/LoadBalancer/g' | kubectl apply -f -
```

2. Get the svc external ip
```
kubectl get svc grafana zipkin -n nginx-mesh
```

{{< output >}}
NAME      TYPE           CLUSTER-IP     EXTERNAL-IP                                                                 PORT(S)          AGE
grafana   LoadBalancer   172.20.176.7   a85065a76e142478fb29cdd2f2904e82-320541984.eu-central-1.elb.amazonaws.com   3000:31309/TCP   4d23h
zipkin    LoadBalancer   172.20.88.44   a7c02ef8a39b74da4ad41085261859f8-765188493.eu-central-1.elb.amazonaws.com   9411:31661/TCP   4d23h
{{< /output >}}

3. Browse to the Grafana dashboard `http://<GRAFANA-EXTERNAL-IP>:3000`

You can get a general look for you application status. The Grafana dashboards can be customized to your hearts desire.

4. Go and buy or sell crypto coin. We will use Zipkin to trace this operation through the different services.
![](/images/050_040_2.JPG)



4. Browse to the Zipkin dashboard `http://<ZIPKIN-EXTERNAL-IP>:9411`
Create a filter as in the bellow image.
Click on the result that looks like the first entry.
![](/images/050_040_1.JPG)

5. Explanation of what is going on