# Istio

# Install Helm

```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz
tar -xzvf helm-v2.9.1-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm

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
curl http://a69300426e81b11e8864a0ee2c9ec821-874261291.us-east-1.elb.amazonaws.com -H "Host: hello-tls.example.com"
```
# How to test mutual tls is working
```
kubectl get secret istio.default -o jsonpath='{.data.root-cert\.pem}' | base64 --decode > root-cert.pem
kubectl get secret istio.default -o jsonpath='{.data.cert-chain\.pem}' | base64 --decode > cert-chain.pem
kubectl get secret istio.default -o jsonpath='{.data.key\.pem}' | base64 --decode > key.pem

kubectl get pods

kubectl cp root-cert.pem mtlstest-697c7b4b7c-g5hgl:/tmp -c mtlstest
kubectl cp cert-chain.pem mtlstest-697c7b4b7c-g5hgl:/tmp -c mtlstest
kubectl cp key.pem mtlstest-697c7b4b7c-g5hgl:/tmp -c mtlstest

kubectl exec -ti mtlstest-697c7b4b7c-g5hgl /bin/bash

mkdir /etc/certs
mv /tmp/*.pem /etc/certs/

curl -k -v http://hello.ns1.svc.cluster.local:8080/hello

```
