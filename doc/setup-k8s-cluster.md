### Setup K8s cluster

Execute the following steps to setup a standard Kubernetes cluster for the deployment of the communication-aware scheduler:

- Export KUBECONFIG environment variable
```
export KUBECONFIG=/home/angelo/scheduler-plugins-project/admin.conf
```

- Deploy mentat app
```
kubectl apply -f mentat.yaml
```

- Deploy Dgraph database
```
kubectl apply -f dgraph.yaml
```

- Install Istio
```
istioctl install --set profile=demo -y
```

- Label default namespace
```
kubectl label namespace default istio-injection=enabled
```

- Setup addons
```
kubectl apply -f istio-addons
```

- Enable access to addons
```
istioctl dashboard prometheus
istioctl dashboard kiali
istioctl dashboard jaeger
istioctl dashboard grafana
```

- Clean up
```
kubectl delete -f istio-addons
istioctl manifest generate --set profile=demo | kubectl delete --ignore-not-found=true -f -
kubectl delete namespace istio-system
kubectl label namespace default istio-injection-
kubectl delete -f mentat.yaml
kubectl delete -f dgraph.yaml
```
