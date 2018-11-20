# Istio

# Install Helm

```
kubectl create -f https://raw.githubusercontent.com/k8s-class/helm/master/helm-rbac.yaml
kubectl create -f https://raw.githubusercontent.com/k8s-class/helm/master/helm-cluster-admin.yaml
helm init --service-account tiller
```


# Install Istio
```
git clone https://github.com/istio/istio.git
helm install install/kubernetes/helm/istio --name istio --namespace istio-system --set tracing.enabled=true,servicegraph.enabled=true,grafana.enabled=true
```
