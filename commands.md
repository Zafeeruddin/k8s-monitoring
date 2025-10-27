# Setup

1. Install prometheus stack

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f custom-values.yaml
```


2. Setup nginx to access


```
sudo mv ~/nginx-conf.yml /etc/nginx/sites-available
sudo ln -s /etc/nginx/sites-available/nginx-conf.yml /etc/nginx/sites-enabled/nginx-conf.yml
sudo nginx -t && sudo systemctl reload nginx
```

3. Login in Grafana


username: ```admin```
password: ```prom-operator```



4. Update node alerts

```
kubectl apply -f node-alerts.yaml
```


5. Update alert manager

Extract current values

```
kubectl get secret alertmanager-kube-prometheus-stack-alertmanager -n default -o jsonpath="{.data.alertmanager\.yaml}" | base64 -d > alertmanager.yaml
```

Update alertmanager

```
kubectl create secret generic alertmanager-kube-prometheus-stack-alertmanager \
  -n default \
  --from-file=alertmanager.yaml \
  --dry-run=client -o yaml | kubectl apply -f -
```


6. Integrate teams

Create a webhook from teams channel


Replace it with ```PROM2TEAMS_CONNECTOR``` in ```prom2teams.yaml```


