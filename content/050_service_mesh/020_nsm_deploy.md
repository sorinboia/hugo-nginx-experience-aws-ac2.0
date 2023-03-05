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
virtualserver.k8s.nginx.org/arcadia configured
ubuntu@jumphost:~/lab$ kubectl apply -f files/5service_mesh/zipkin.yaml
namespace/zipkin created
deployment.apps/zipkin created
service/zipkin created
ubuntu@jumphost:~/lab$ nginx-meshctl deploy --sample-rate 1 --tracing-backend "zipkin" --tracing-address "zipkin.zipkin.svc.cluster.local:9411" --persistent-storage off --disable-auto-inject
Flag --sample-rate has been deprecated, and will be removed in a future release. OpenTelemetry (--sampler-ratio and --telemetry-exporters) is the preferred way to configure tracing.
Flag --tracing-backend has been deprecated, and will be removed in a future release. OpenTelemetry (--telemetry-exporters) is the preferred way to configure tracing.
Flag --tracing-address has been deprecated, and will be removed in a future release. OpenTelemetry (--telemetry-exporters) is the preferred way to configure tracing.
Warning: Deploying without persistent storage, not suitable for production environments.
         For production environments ensure a default StorageClass is set.
Deploying NGINX Service Mesh...
All resources created. Testing the connection to the Service Mesh API Server...
Connected to the NGINX Service Mesh API successfully.
NGINX Service Mesh is running.
{{< /output >}}

2. All components have been deployed in the "nginx-mesh" namespace, run the following command and validate that all pods are running
```
kubectl get pods -n nginx-mesh
```
{{< output >}}
NAME                                  READY   STATUS    RESTARTS   AGE
nats-server-f46dfc64-5nndp            2/2     Running   0          96s
nginx-mesh-api-7b64b4798f-8c7zz       1/1     Running   0          96s
nginx-mesh-metrics-86489d7f94-nt6nq   1/1     Running   0          96s
spire-agent-zv49g                     1/1     Running   0          96s
spire-server-555d847594-hgqgv         2/2     Running   0          96s
{{< /output >}}

