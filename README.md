# k8s-humio

Simple helm chart for getting humio up and running in a minikube kubernetes cluster.

**Be aware. This is not meant for production usage.**

## Prerequisites

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

That's all needed to get Humio running.

### Ship logs with fluent-bit from your cluster to humio

When you deployed humio, you exposed two ports. One port is being exposed as `type=LoadBalancer` which enables you to access humio on a specific nodeport. Further the deployment created a service: `k8s-humio-es`. This service is used to ship logs to humios elasticsearch api. 

To ship logs from your cluster. Go back to the project root. Edit the file `humio-agent.yaml` and insert the ingest token from you humio dataspace (docs: https://docs.humio.com/sending-data-to-humio/ingest-tokens/). You can just create a new dataspace and the token should be easy to find.

Insert the token in the `backend.es.http_user`.

Now deploy the fluent-bit `DaemonSet` using the official helm chart.

```
helm install stable/fluent-bit --name=humio-agent -f humio-agent.yaml --set on_minikube=true 
```

You should now be able to see logs coming into your dataspace in Humio.


**Again, this is not ment for production usage, since it stores data on the minikube host.** 

## Future development?
* Support switching between environments
* Support different storage options
* More Humio configuration options 
* Migrate to StatefulSet's
* Probably much more stuff
* ?

## Contribution
If you find bugs, want to add more features, etc. Please submit PR's or create issues.

