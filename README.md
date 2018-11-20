# Istio

### Install Istio
```
git clone https://github.com/istio/istio.git
helm install install/kubernetes/helm/istio --name istio --namespace istio-system --set tracing.enabled=true,servicegraph.enabled=true,grafana.enabled=true
```
