# GKE Ingress for external HTTP(S) load balancers

This page provides a general overview of what Ingress for external HTTP(S) load balancers is and how it works. Google Kubernetes Engine (GKE) provides a built-in and managed Ingress controller called GKE Ingress. This controller implements Ingress resources as Google Cloud load balancers for HTTP(S) workloads in GKE.

## What is Ingress?

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

Here is a simple example where an Ingress sends all its traffic to one Service: 

![](what-is-ingress.svg)


