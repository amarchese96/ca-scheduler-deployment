### Build and deploy the communication aware scheduler

After setting up a Kubernetes cluster, execute the following steps inside the *angelo1996/scheduler-plugins* project to build and deploy the communication-aware scheduler:

- Build scheduler docker image
```
cd scheduler-plugins
make local-image LOCAL_REGISTRY=docker.io/angelo1996 LOCAL_IMAGE=kube-scheduler:latest
```

- Push to remote registry
```
docker push docker.io/angelo1996/kube-scheduler:latest
```

- Export KUBECONFIG environment variable
```
export KUBECONFIG=/home/angelo/scheduler-plugins-project/admin.conf
```

- Deploy scheduler with Helm
```
cd manifests/install/charts
helm install scheduler-plugins as-a-second-scheduler/
```

- Clean up
```
helm uninstall scheduler-plugins
```