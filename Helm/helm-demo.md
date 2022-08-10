## Helm demo
*This demo will user the ingress on bare-metal using the Nginx Ingress Controller and metalLB.*

In my `helm-demo` folder, I have the `templates` directory and the `Chart` and `values` YAML files :
```bash
justk8s-master@master:~/helm-demo$ ls
Chart.yaml  templates  values.yaml
```
Inside the Chart.yaml file :
```YAML
name: my-first-demo
version: 1.0.0
description: easy helm demo
maintainers:
- name: Siwar
```
This is the metadata to describe each deployment.

Now inside the templates directory :
```bash
justk8s-master@master:~/helm-demo/templates$ ls
deployment.yaml  ingress.yaml  service.yaml
```
The depoyment.yaml file contains a simple nginx deployment :
```YAML
apiVersion: v1
kind: Deployment
metadata: 
  name: my-nginx-deployment
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
And the service.yaml file :
``` YAML
apiVersion: v1
kind: Service
metadata: 
  name: my-nginx-service
spec:
  selector:
    app: nginx
  ports:
  - name: main
    protocol: TCP
    port: 80
    targetPort: 8080
```
And the ingress.yaml :
```YAML
apiVersion: v1
kind: Ingress
metadata:
  name: my-nginx-ingress
  annotations:
    http.port: "443"
spec:
  backend:
    serviceName: nginx
    servicePort: 80
```
