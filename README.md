# Istio

# Install Helm

```
kubectl create -f https://raw.githubusercontent.com/k8s-class/helm/master/helm-rbac.yaml
kubectl create -f https://raw.githubusercontent.com/k8s-class/helm/master/helm-cluster-admin.yaml
helm init --service-account tiller
```


# Install Istio
```
curl -L https://git.io/getLatestIstio | sh -
cd istio-1.0.X
helm install install/kubernetes/helm/istio --name istio --namespace istio-system --set tracing.enabled=true,servicegraph.enabled=true,grafana.enabled=true --set global.mtls.enabled=true


```
# Get Hello World !!! up and running
```
kubectl create -f <(istioctl kube-inject -f helloworld-tls.yaml) # Needs Sidecar for mTLS
kubectl create -f create -f helloworld-tls-legacy.yaml # Legacy does not use mTLS
kubectl create -f disable-tls-legacy.yaml
kubectl get services --all-namespaces | grep istio-ingressgateway ( get external IP Address )

curl http://168.61.165.163 /hello -H "Host: hello-tls.example.com"

```
