## Import admin.conf file inside root path of the project from kubernetes cluster master node

## Export KUBECONFIG environment variable
```
export KUBECONFIG=/home/angelo/scheduler-plugin-project/admin.conf
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

## Clean up
```
kubectl delete -f istio-addons
istioctl manifest generate --set profile=demo | kubectl delete --ignore-not-found=true -f -
kubectl delete namespace istio-system
kubectl label namespace default istio-injection-
kubectl delete -f mentat.yaml
```
