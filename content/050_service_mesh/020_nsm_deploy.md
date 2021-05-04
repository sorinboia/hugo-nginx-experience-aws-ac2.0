+++
title = "Nginx Service Mesh deployment"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++



1. Deploy all Nginx components in the Kubernetes environment

```
nginx-meshctl deploy --registry-server "sorinboia" --image-tag 0.9.0 --sample-rate 1 --disable-auto-inject
```
{{< output >}}
Deploying NGINX Service Mesh Control Plane in namespace "nginx-mesh"...
Created namespace "nginx-mesh".
W0502 08:20:37.942849    7607 warnings.go:70] apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
Created SpiffeID CRD.
W0502 08:20:38.076108    7607 warnings.go:70] admissionregistration.k8s.io/v1beta1 ValidatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 ValidatingWebhookConfiguration
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

