### Setup K8s cluster

Execute the following steps to setup a standard Kubernetes cluster for the deployment of the communication-aware scheduler:

- Export KUBECONFIG environment variable
```
export KUBECONFIG=/path/to/admin.conf
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

- Install prometheus server
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus -n istio-system --values observability/prometheus-values.yml
```

- Install grafana dashboard
```
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana -n istio-system --values observability/grafana-values.yml
```

- Install jaeger
```
kubectl apply -f observability/jaeger.yml -n istio-system
```

- Install kiali
```
kubectl apply -f observability/kiali.yml -n istio-system
```

- Clean up
```
kubectl delete -f observability/kiali.yml -n istio-system
kubectl delete -f observability/jaeger.yml -n istio-system
helm uninstall grafana -n istio-system
helm uninstall prometheus -n istio-system
istioctl manifest generate --set profile=demo | kubectl delete --ignore-not-found=true -f -
kubectl delete namespace istio-system
kubectl label namespace default istio-injection-
kubectl delete -f mentat.yaml
kubectl delete -f dgraph.yaml
```
