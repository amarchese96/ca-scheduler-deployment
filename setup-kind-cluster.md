## Create kind cluster
```
kind create cluster --config kind-cluster-config.yaml
```

## Deploy mentat app
```
kubectl apply -f mentat.yaml
```

## Deploy Dgraph database
```
kubectl apply -f dgraph.yaml
```

## Install istio
```
istioctl install --set profile=demo -y
```

## Label default namespace
```
kubectl label namespace default istio-injection=enabled
```

## setup addons
```
kubectl apply -f istio-addons
```

## enable access to addons
```
istioctl dashboard prometheus
istioctl dashboard kiali
istioctl dashboard jaeger
istioctl dashboard grafana
```

## Delete kind cluster
```
kind delete cluster --name kind
```