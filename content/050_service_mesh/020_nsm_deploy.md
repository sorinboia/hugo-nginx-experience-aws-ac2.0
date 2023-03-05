+++
title = "Nginx Service Mesh deployment"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++



1. Deploy all Nginx components in the Kubernetes environment

```
kubectl apply -f files/5service_mesh/zipkin.yaml
nginx-meshctl deploy --sample-rate 1 --tracing-backend "zipkin" --tracing-address "zipkin.zipkin.svc.cluster.local:9411" --persistent-storage off --disable-auto-inject
```
{{< output >}}
Deploying NGINX Service Mesh Control Plane in namespace "nginx-mesh"...
Created namespace "nginx-mesh".
W1116 07:38:27.529470    3517 warnings.go:70] apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
Created SpiffeID CRD.
W1116 07:38:27.834807    3517 warnings.go:70] admissionregistration.k8s.io/v1beta1 ValidatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 ValidatingWebhookConfiguration
Waiting for SPIRE to be running...done.
Deployed Spire.
Deployed NATS server.
Created traffic policy CRDs.
Deployed Mesh API.
Deployed Metrics API Server.
Deployed Prometheus Server nginx-mesh/prometheus.
Deployed Grafana nginx-mesh/grafana.
Deployed tracing server nginx-mesh/zipkin.
All resources created. Testing the connection to the Service Mesh API Server...
Connected to the NGINX Service Mesh API successfully.
Verifying that all images were pulled...done.
NGINX Service Mesh is running.
ubuntu@jumphost:~/lab$ kubectl get svc -n nginx-mesh grafana -oyaml | sed 's/ClusterIP/LoadBalancer/g' | kubectl apply -f -
Warning: kubectl apply should be used on resource created by either kubectl create --save-config or kubectl apply
service/grafana configured
ubuntu@jumphost:~/lab$ kubectl get svc -n nginx-mesh zipkin -oyaml | sed 's/ClusterIP/LoadBalancer/g' | kubectl apply -f -
Warning: kubectl apply should be used on resource created by either kubectl create --save-config or kubectl apply
service/zipkin configured
{{< /output >}}

2. All components have been deployed in the "nginx-mesh" namespace, run the following command and validate that all pods are running
```
kubectl get pods -n nginx-mesh
```
{{< output >}}
grafana-766d495d65-mjngv              1/1     Running   0          5m30s
nats-server-7d457c74d9-csjt2          1/1     Running   0          5m34s
nginx-mesh-api-575858b96b-djq7h       1/1     Running   0          5m32s
nginx-mesh-metrics-57dd4796b8-6mtqc   1/1     Running   0          5m31s
prometheus-67dc46b8b6-2wkgg           1/1     Running   0          5m30s
spire-agent-nv5z2                     1/1     Running   0          6m18s
spire-server-0                        2/2     Running   0          6m18s
zipkin-564b9d4954-4hqdr               1/1     Running   0          5m29s
{{< /output >}}

