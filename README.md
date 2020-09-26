# All in One Scripts

```sh
cat << EOF > kind-2nodes.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
  - containerPort: 30080
    hostPort: 80
  - containerPort: 30443
    hostPort: 443
EOF
```

```sh
kind create cluster --config ./kind-2nodes.yaml
```

```sh
kind export kubeconfig
```

```sh
kubectl cluster-info
kubectl get nodes
```

```sh
cat <<EOF > mynginx.yaml
apiVersion: v1
kind: Service
metadata:
  name: web-nginx
  labels:
    app.kubernetes.io/name: nginx
spec:
  type: NodePort
  ports:
    - name: http
      port: 80
      targetPort: http
      nodePort: 30080
  selector:
    app.kubernetes.io/name: nginx
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-nginx
  labels:
    app.kubernetes.io/name: nginx
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app.kubernetes.io/name: nginx
    spec:
      containers:
        - name: nginx
          image: docker.io/bitnami/nginx:1.19.1-debian-10-r23
          ports:
            - name: http
              containerPort: 8080
EOF
```

```sh
kubectl apply -f mynginx.yaml
```

```sh
kubectl delete -f ./mynginx.yaml
```

```sh
curl -sL https://istio.io/downloadIstio | sh -
cd istio-1.*
export PATH=$PWD/bin:$PATH
```

```sh
istioctl x precheck
```

```sh
istioctl install --set profile=demo
```

```sh
kubectl label namespace default istio-injection=enabled
```

```sh
kubectl get namespace -L istio-injection
```

```sh
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

kubectl get services

kubectl get pods

kubectl exec -it $(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}') -c ratings -- curl productpage:9080/productpage | grep -o "<title>.*</title>"
```

```sh
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

```sh
istioctl analyze
```

```sh
kubectl get svc istio-ingressgateway -n istio-system
```

```sh
kubectl apply -f samples/addons/prometheus.yaml -f samples/addons/grafana.yaml
```

```sh
istioctl dashboard grafana
```

```sh
kubectl apply -f samples/addons/kiali.yaml
kubectl apply -f samples/addons/kiali.yaml
```

```sh
istioctl dashboard kiali
```

```sh
kubectl exec -it $(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}') -c ratings -- bash
curl -s productpage:9080/productpage | grep -o "<title>.*</title>"
```

```sh
while true; do curl -s productpage:9080/productpage | grep -o "<title>.*</title>"; done
```
