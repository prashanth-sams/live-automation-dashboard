# Live Automation Dashboard
> A single dashboard to monitor live automation results
## Grafana

```
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