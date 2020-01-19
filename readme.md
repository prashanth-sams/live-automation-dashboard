# Live Automation Dashboard
> A single dashboard to monitor live automation results
## Grafana

```
cd grafana/
helm install --name grafana . --namespace=qa-dashboard
```

### Node Port
```
kubectl get --namespace qa-dashboard -o jsonpath="{.spec.ports[0].nodePort}" services grafana ; echo
```
### Node IP
```
kubectl get nodes --namespace qa-dashboard -o jsonpath="{.items[0].status.addresses[0].address}" ; echo
```

### Access grafana service
```
http://$NODE_IP:$NODE_PORT
```
> username:

`admin`

> password:
```
kubectl get secret --namespace qa-dashboard grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

### Frequent commands:
```
kubectl get pods --namespace=qa-dashboard -o wide

helm delete grafana

helm del $(helm ls --all | grep 'DELETED' | awk '{print $1}') --purge
```

## Prometheus
```
cd prometheus/
helm install --name prometheus . --namespace=qa-dashboard
```

### Prometheus server
```
export POD_NAME=$(kubectl get pods --namespace qa-dashboard -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
```
```
kubectl --namespace qa-dashboard port-forward $POD_NAME 9090
```
> http://localhost:9090/

### PushGateway
```
export POD_NAME=$(kubectl get pods --namespace qa-dashboard -l "app=prometheus,component=pushgateway" -o jsonpath="{.items[0].metadata.name}")
```
```
kubectl --namespace qa-dashboard port-forward $POD_NAME 9091
```
> http://localhost:9091/

### Alertmanager
```
export POD_NAME=$(kubectl get pods --namespace qa-dashboard -l "app=prometheus,component=alertmanager" -o jsonpath="{.items[0].metadata.name}")
```
```
kubectl --namespace qa-dashboard port-forward $POD_NAME 9093
```
> http://localhost:9093/