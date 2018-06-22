# k8s-humio

Simple helm chart for getting humio up and running in a minikube kubernetes cluster.

**Be aware. This is not meant for production usage.**

## Prerequisuites

In order to get this up and running, you need:
* `minikube` (link: https://github.com/kubernetes/minikube)
* `helm` (link: https://github.com/kubernetes/helm)

## Installation

This is really simple. Start by spinning up a `minikube` cluster:

```
minikube --memory 6144 start
```
I specify the `memory` argument to make sure we have enough. 
Once the minikube cluster is up an running, you need to install tiller, which is the server-side component of helm:
```
helm init
```
Now, the last thing is to install humio in the cluster. Go to the chart directory.
```
cd helmchart/k8s-humio
```
and install the chart
```
helm install . --name humio
```

This will spin up humio as a deployment in your minikube cluster, and further expose it as a service. You can now access the humio ui:
```
minikube service k8s-humio
```

That's all.

**Again, this is not ment for production usage, since it stores data on the minikube host.** 

## Future development?
* Support switching between environments
* More Humio configuration options 
* Migrate to StatefulSet's
* ?

## Contribution
If you find bugs, want to add more features, etc. Please submit PR's or create issues.

