# Configure Ingress for external load balancing

This page shows you how to configure an external HTTP(S) load balancer by creating a Kubernetes Ingress object.

## Enable the HttpLoadBalancing add-on

Your cluster must have the HttpLoadBalancing add-on enabled. This add-on is enabled by default. In Autopilot clusters, you can't disable this add-on.

```
gcloud container clusters update backyard-gke-tracing --update-addons=HttpLoadBalancing=ENABLED
```

## Reserve a global static external IP address

```
gcloud compute addresses create backyard-gke-tracing-external-lb-ip --project=innovation-lab-devops --global
```

## Create Deployments and Services

Create two Deployments with Services named `hello-world-1` and `hello-world-2`:

1. Save the following manifest as `hello-world-deployment-1.yaml`:

## Create an Ingress

Create an Ingress that specifies rules for routing requests depending on the URL path in the request. When you create the Ingress, the GKE Ingress controller creates and configures an external HTTP(S) load balancer.

```
kubectl apply -f my-ingress.yaml
```

## Test the external HTTP(S) load balancer

Wait about five minutes for the load balancer to be configured, then test the external HTTP(S) load balancer:

**View the Ingress:**

```
kubectl get ingress my-ingress --output yaml
```

**Test hello-world-1**

```
curl 34.149.197.196
```

**Test hello-world-2**
```
curl 34.149.197.196/v2
```
