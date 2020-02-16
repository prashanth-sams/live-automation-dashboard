# Live Automation Dashboard
> A single dashboard to monitor live automation results
## **Grafana**
```
cd grafana/
helm install --name grafana . --namespace monitoring --set rbac.create=false

helm3 install grafana . -n monitoring --set rbac.create=false
```

### Access grafana service
By default, the username credentials are set in this chart.
```
username: admin
password: admin
```
In-case if you remove password, you can retrieve it from:
```
kubectl get secret --namespace qa-dashboard grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
### Port forward Grafana
```
kubectl --namespace monitoring port-forward $(kubectl get pods -n monitoring | grep 'grafana' | awk '{print$1}') 3000
```
Grafana access URL: http://localhost:3030/

## **Prometheus**
```
cd prometheus/
helm install --name prometheus . --namespace monitoring --set rbac.create=false

helm3 install dashboard . -n monitoring --set rbac.create=false
```

### Port forward Prometheus server
```
kubectl --namespace monitoring port-forward $(kubectl get pods -n monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}") 9090

kubectl --namespace monitoring port-forward $(kubectl get pods -n monitoring | grep 'server' | awk '{print$1}') 9090
```
Prometheus access URL: http://localhost:9090/

### Port forward PushGateway
```
kubectl --namespace monitoring port-forward $(kubectl get pods -n monitoring -l "app=prometheus,component=pushgateway" -o jsonpath="{.items[0].metadata.name}") 9091

kubectl --namespace monitoring port-forward $(kubectl get pods -n monitoring | grep 'pushgateway' | awk '{print$1}') 9091
```
PushGateway access URL: http://localhost:9091/

### Port forward Alertmanager
```
kubectl --namespace monitoring port-forward $(kubectl get pods --namespace monitoring -l "app=prometheus,component=alertmanager" -o jsonpath="{.items[0].metadata.name}") 9093
```
Alertmanager access URL: http://localhost:9093/


## Frequently used commands:
```
kubectl get pods -n monitoring -o wide
kubectl get svc -n monitoring

helm delete grafana -n monitoring
helm3 del grafana -n monitoring
helm3 del dashboard -n monitoring

helm del $(helm ls --all | grep 'DELETED' | awk '{print $1}') --purge
```