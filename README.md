# Sysdig Memory Test

Repo provides a series of scenarios to identify out of memory issues in pods. <br/>
You specify minimum and maximum memory values in a ```LimitRange``` object. <br/>
<br/>
Firstly, create a namespace so that the resources you create in this exercise are isolated from the rest of your cluster. <br/>
If a Pod does not meet the constraints imposed by the LimitRange, it cannot be created in the namespace.

```
kubectl create namespace constraints-mem-example
```

Create the LimitRange within the network namespace ```constraints-mem-example```. <br/>
Verify that every container in that Pod requests no more than 50 MiB of memory.

```
kubectl apply -f https://raw.githubusercontent.com/n1g3ld0uglas/sysdig-memory-test/main/memory-constraints.yaml --namespace=constraints-mem-example
```

View detailed information about the LimitRange:
```
kubectl get limitrange mem-min-max-demo-lr --namespace=constraints-mem-example --output=yaml
```

The output shows the minimum and maximum memory constraints as expected. <br/>
Even though you didn't specify default values in the configuration file for the LimitRange, they were created automatically.

```
spec:
  limits:
  - default:
      memory: 50Mi
    defaultRequest:
      memory: 50Mi
    max:
      memory: 50Mi
    type: Container
```

Create the boutique test application in the namespace ```constraints-mem-example```. <br/>
This way those constraints will apply specifically to this newly-introduce application only.

```
kubectl apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/microservices-demo/master/release/kubernetes-manifests.yaml -n constraints-mem-example
```  
