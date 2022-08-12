## Ingress
In Kubernetes, Ingress is used to configure routing rules to different services of your application. This also offers a public user-friendly URL for the end-users.

But the Ingress is just routing rules, Kubernetes requires an Ingress Controller that grabs these routing rules and configures the Load Balancer dynamically in your K8s cluster.

In cloud environments like AWS, Azure, GCP, etc., the Ingress Controller is supplied by the cloud provider.

In bare-metal K8s cluster, unless we deploy an Ingress Controller into our Kubernetes cluster, the Ingress will not work.

## Bare metal considerations
In traditional cloud environments, where network load balancers are available on-demand, a single Kubernetes manifest suffices to provide a single point of contact to the NGINX Ingress controller to external clients and, indirectly, to any application running inside the cluster. 


<img src="ingressCloud.png" width=50% height=50%>

Bare-metal environments lack this commodity, requiring a slightly different setup to offer the same kind of access to external consumers.

<img src="ingressBare.png"  width=50% height=50%>

## A pure software solution: MetalLB
MetalLB provides a network load-balancer implementation for Kubernetes clusters that do not run on a supported cloud provider, effectively allowing the usage of LoadBalancer Services within any cluster.

This figure demonstrates how to use the Layer 2 configuration mode of MetalLB together with the NGINX Ingress controller in a Kubernetes cluster that has publicly accessible nodes. In this mode, one node attracts all the traffic for the ingress-nginx Service IP.


<img src="metalLB.png"  width=50% height=50%>


*Reference : [Nginx Ingress Controller](https://kubernetes.github.io/ingress-nginx/deploy/baremetal/)*

## Installing MetalLB 
MetalLB can be deployed either with a simple Kubernetes manifest or with Helm.

### Installation With Manifest 

```bash
justk8s-master@master:~$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/namespace.yaml
justk8s-master@master:~$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/metallb.yaml

```

Now we must configure the metalLB using configmap object. In this configmap we give the address pool that will be used as LoadBalancer IP! Also the type of the Load Balancer (metalLB supports two LoadBalancing modes : Layer 2 mode and BGP mode. In our case we will use the Layer 2 mode).

Create the ConfigMap manifest containing metalLB configuration:
```YAML 
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.1.240-192.168.1.250
```
And then apply it :
```bash 
justk8s-master@master:~$ kubectl apply -f metallb-configmap.yaml
configmap/config created
```

Reference : *[Installation Guide](https://metallb.universe.tf/installation/)*

## Install NGINX Ingress Controller
There are multiple ways to install the NGINX ingress controller:

- with Helm, using the project repository chart;
- with kubectl apply, using YAML manifests;
- with specific addons (e.g. for minikube or MicroK8s).

### Using the YAML manifest file
```bash
justk8s-master@master:~$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
serviceaccount/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
configmap/ingress-nginx-controller created
service/ingress-nginx-controller created
service/ingress-nginx-controller-admission created
deployment.apps/ingress-nginx-controller created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
ingressclass.networking.k8s.io/nginx created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
```
After waiting a few minute until the controller is ready, we must check all the services and deployments of the ingress-nginx namespace again ! You must find an output similar to the following :

```bash
justk8s-master@master:~$ kubectl get all -n ingress-nginx
NAME                                            READY   STATUS      RESTARTS   AGE
pod/ingress-nginx-admission-create-gnp2c        0/1     Completed   0          37s
pod/ingress-nginx-admission-patch-s8drh         0/1     Completed   1          37s
pod/ingress-nginx-controller-7575567f98-42qdg   1/1     Running     0          37s

NAME                                         TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                      AGE
service/ingress-nginx-controller             LoadBalancer   10.110.78.165   192.168.1.240   80:32399/TCP,443:32414/TCP   40s
service/ingress-nginx-controller-admission   ClusterIP      10.107.6.130    <none>          443/TCP                      39s

NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ingress-nginx-controller   1/1     1            1           39s

NAME                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/ingress-nginx-controller-7575567f98   1         1         1       38s

NAME                                       COMPLETIONS   DURATION   AGE
job.batch/ingress-nginx-admission-create   1/1           13s        38s
job.batch/ingress-nginx-admission-patch    1/1           15s        38s
```
Finally, the ingress controller is ready and on running state! also is exposed with Load Balancer and it has an Extenal-IP `192.168.1.240`.

Now we can deploy an ingress object without any problem ! 
