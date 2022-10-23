---
title: "How to add dependency in Helm"
date: "2021-03-07"
categories: 
  - "helm"
---

In this blog, we will be deploying [this](https://kubernetes.io/docs/tutorials/stateless-application/guestbook/) guestbook application using helm and add the `Redis` as a dependency.

Checkout my [Youtube](https://youtu.be/6HrigAR0m5Q) video on this blog

![](https://cdn-images-1.medium.com/max/1600/1*pHAMUkjw6N_1-8PUzRPZyw.png)

### **Prerequisite**

1. Kubernetes Cluster Setup
2. Clone [this](https://github.com/devops4solutions/guestbook) git repo

### **Setup a helm project**

helm create guestbook  
rm -rf guestbook/templates/tests

### **Adding a Redis Chart dependency**

Chart dependencies are used to install other charts’ resources that a Helm chart may depend on. 

In this example, we are using Redis as a database so we to need add this as a dependency.

First we can search the charts for `redis`

**helm search hub redis**

Now we will add the dependency section in the `Charts.yaml` file

dependencies:  
  - name: redis  
    version: 12.7.x  
    repository: [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)

### **How to download this dependency?**

![](https://cdn-images-1.medium.com/max/1600/1*QWiJqpEoVuK8piDdn6rT_A.png)

When downloading a dependency for the first time, you should use the **helm dependency update** command. 

This command will download your dependency to the **charts/** directory and will generate the **Chart.lock** file, which specifies metadata about the chart that was downloaded.

helm dependency update guestbook

![](https://cdn-images-1.medium.com/max/1600/1*446lgblszhzNU7uVcnYOsg.png)

Now you should see that it has downloaded the dependency and also updated the Chart.lock file

![](https://cdn-images-1.medium.com/max/1600/1*zNawgUX-GBiDsbkWygJvLQ.png)

![](https://cdn-images-1.medium.com/max/1600/1*A61ZujBZBfBLaFPF-HNxrw.png)

### **ADDING VALUES TO CONFIGURE THE REDIS CHART**

You can override the default values of Redis chart using values.yaml file

![](https://cdn-images-1.medium.com/max/1600/1*RHZjpCyYy9vDkgSaY9hSWQ.png)

### **Modify frontend application** 

By default it has created these templates

![](https://cdn-images-1.medium.com/max/1600/1*4i5sszc19R1r2ke-zRi9Nw.png)

- **deployment.yaml**: Used to deploy the Guestbook application to Kubernetes.
- **ingress.yaml**: Provides one option to access the Guestbook application from outside the Kubernetes cluster.
- **serviceaccount.yaml**: Used to create a dedicated **serviceaccount** for the Guestbook application.
- **service.yaml**: Used to load-balance between multiple instances of the Guestbook application. Can also provide an option to access the Guestbook application from outside the Kubernetes cluster.
- **\_helpers.tp**: Provides a set of common templates used throughout the Helm chart.
- **NOTES.txt**: Provides a set of instructions used to access the application after it is installed.

In the `values.yaml` file you can change the default image to the one which you want to use for your application

![](https://cdn-images-1.medium.com/max/1600/1*N1Uas1k987bMZkgMriX0DA.png)

Now we will update this image as per our requirement

![](https://cdn-images-1.medium.com/max/1600/1*Y4EUDiYjG0cn8CycXcmhFw.png)

Also, change the NodePort so that we can browse the application

![](https://cdn-images-1.medium.com/max/1600/1*IHithVUjlFnNO3phPR-Lyg.png)

### **Install the chart**

kubectl create ns gb( Create the namespace first)

helm install my-gb guestbook -n gb  
kubectl get pods -n gb

![](https://cdn-images-1.medium.com/max/1600/1*Xe2t4zrFzFG2U6hwrF4aiA.png)

It has created `PVC` also as shown below

![](https://cdn-images-1.medium.com/max/1600/1*19B5UiIOcShpk4o0QPqCQA.png)

export NODE\_PORT=$(kubectl get --namespace helm1 -o jsonpath="{.spec.ports\[0\].nodePort}" services frontend-demo-helm)

export NODE\_IP=$(kubectl get nodes --namespace helm1 -o jsonpath="{.items\[0\].status.addresses\[0\].address}")

echo [http://$NODE\_IP:$NODE\_PORT](http://$NODE_IP:$NODE_PORT)

![](https://cdn-images-1.medium.com/max/1600/1*Xm-c0X2w9hSJseVN4iRYTA.png)

### **Uninstall the chart**

helm uninstall my-gb -n gb  
kubectl delete pvc -l app=redis -n gb

![](https://cdn-images-1.medium.com/max/1600/1*cz62rufZHcnO-_-TawCIUw.png)
