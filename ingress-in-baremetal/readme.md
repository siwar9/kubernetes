## Ingress
In Kubernetes, Ingress is used to configure routing rules to different services of your application. This also offers a public user-friendly URL for the end-users.

But the Ingress is just routing rules, Kubernetes requires an Ingress Controller that grabs these routing rules and configures the Load Balancer dynamically in your K8s cluster.

In cloud environments like AWS, Azure, GCP, etc., the Ingress Controller is supplied by the cloud provider.

In bare-metal K8s cluster, unless we deploy an Ingress Controller into our Kubernetes cluster, the Ingress will not work.

## Bare metal considerations
In traditional cloud environments, where network load balancers are available on-demand, a single Kubernetes manifest suffices to provide a single point of contact to the NGINX Ingress controller to external clients and, indirectly, to any application running inside the cluster. Bare-metal environments lack this commodity, requiring a slightly different setup to offer the same kind of access to external consumers.
![Figure1](ingressCloud.png)
![Figure2](ingressBare.png)

## A pure software solution: MetalLB
MetalLB provides a network load-balancer implementation for Kubernetes clusters that do not run on a supported cloud provider, effectively allowing the usage of LoadBalancer Services within any cluster.

This section demonstrates how to use the Layer 2 configuration mode of MetalLB together with the NGINX Ingress controller in a Kubernetes cluster that has publicly accessible nodes. In this mode, one node attracts all the traffic for the ingress-nginx Service IP. See Traffic policies for more details.
![Figure3](metalLB.png)
