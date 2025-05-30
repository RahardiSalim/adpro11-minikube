# Hello Minikube - Reflection

## 1. Application Logs Before and After Exposing as a Service

Before exposing the deployment as a Service, accessing the application directly was not possible without port-forwarding or using `minikube service`. As a result, the logs remained mostly unchanged unless I used a direct method to access the pod.

After exposing the app as a Service (using `kubectl expose`), I used `minikube service hello-minikube` to open the application in a browser. I observed that every time I refreshed or reopened the app, a new log entry appeared when I ran:

```sh
kubectl logs <pod-name>
````

This confirmed that each HTTP request to the Service (triggered by accessing the app) was hitting the pod and generating corresponding log output. So yes, the number of logs increased each time I opened or refreshed the app while the Service proxy was running.

This behavior makes sense because the pod is serving traffic via the Service, and each request leads to the container logging activity.

## 2. `kubectl get` With and Without the `-n` Option

During the tutorial, two forms of `kubectl get` commands were used:

* `kubectl get pods` or `kubectl get services`
* `kubectl get pods -n kube-system` or `kubectl get services -n kube-system`

The `-n` option specifies the **namespace** from which to retrieve resources. Kubernetes organizes resources into namespaces to logically separate them. The default namespace (if `-n` is not used) is simply `default`.

The reason the output did not list the pods or services I explicitly created when using `-n kube-system` is because I deployed my app (e.g., `hello-minikube`) into the default namespace, not the `kube-system` one.

### Summary:

* `kubectl get pods` — shows resources in the `default` namespace.
* `kubectl get pods -n kube-system` — shows resources in the `kube-system` namespace (typically reserved for Kubernetes system components like CoreDNS, kube-proxy, etc.).
