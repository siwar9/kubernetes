How do we version control a Deployment?
How do we keep track of them?
This is where Helm comes in
a chart is a group of manifest files (state of Deplolyment that youj have)
Demo :
l have a minikube set up 

## Install Helm

### Using the Binary Releases
Every release of Helm provides binary releases for a variety of OSes. These binary versions can be manually downloaded and installed.

1. Download your desired version
2. Unpack it (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
3. Find the helm binary in the unpacked directory, and move it to its desired destination (mv linux-amd64/helm /usr/local/bin/helm)

### Using Script
Helm has an installer script that will automatically grab the latest version of Helm and install it locally.

You can fetch that script, and then execute it locally. It's well documented so that you can read through it and understand what it is doing before you run it.
```bash
justk8s-master@master:~$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
justk8s-master@master:~$ chmod 700 get_helm.sh
justk8s-master@master:~$ ./get_helm.sh
Downloading https://get.helm.sh/helm-v3.9.2-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
[sudo] password for justk8s-master:
helm installed into /usr/local/bin/helm
``` 
For more installation methods, check out *[the installation guide](https://helm.sh/docs/intro/install/)*.

After installing Helm successfully, you should be able to run the client and add the stable repo.

### Initialize a Helm Chart Repository
Once you have Helm ready, you can add a chart repository.
```bash
justk8s-master@master:~$ helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories
```
Once this is installed, you will be able to list the charts you can install:

```bash
justk8s-master@master:~$ helm search repo bitnami
NAME                                            CHART VERSION   APP VERSION     DESCRIPTION
bitnami/airflow                                 13.0.4          2.3.3           Apache Airflow is a tool to express and execute...
bitnami/apache                                  9.1.17          2.4.54          Apache HTTP Server is an open-source HTTP serve...
bitnami/argo-cd                                 4.0.6           2.4.8           Argo CD is a continuous delivery tool for Kuber...
bitnami/argo-workflows                          2.3.9           3.3.8           Argo Workflows is meant to orchestrate Kubernet...
bitnami/aspnet-core                             3.4.18          6.0.7           ASP.NET Core is an open-source framework for we...
bitnami/cassandra                               9.2.12          4.0.5           Apache Cassandra is an open source distributed ...
#... and many more 
```
Reference : *[Initialize a Helm Chart Repository](https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository)*



