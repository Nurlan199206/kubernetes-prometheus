Software version:

```Ubuntu 18.10```

```Kubernetes: 1.14.1```

```minikube: 1.0.1```

```docker: 18.9.6```

```Prometheus: 2.9.2```

```Grafana: 6.1.6```

```NodeExporter: k8s.gcr.io/cadvisor:v0.30.2```

```node exporter: 0.17.0```

# kubernetes-prometheus
test deployment prometheus with grafana for Kubernetes with 1GB Storage

first create PersistentVolume and PersistentVolumeClaim with commands

1) ```kubectl create -f pv-prom.yaml```

   ```kubectl create -f pv-claim-prom.yaml```

2) ```kubectl apply -f clusterrole.yml```

   ```kubectl apply -f prometheus-deployment-pv.yml```


    change IP in ```prometheus-config-map.yaml```
    
    copy ```prometheus.yml``` to /mnt/prometheus if you wanna use with PV

Create ConfigMap for Grafana

3) ```kubectl create configmap grafana-config --from-file=dashboard.json --from-file=dashboard-provider.yml --from-file=datasource.yaml```


Create PersistentVolume and Claim and Deployment for Grafana.


4) ```kubectl apply -f pv-grafana.yaml```

   ```kubectl apply -f pv-claim-grafana.yaml```

   ```kubectl apply -f grafana-deployment-prod.yaml```
   
   
5) cAdvisor metrics link: https://localhost:10250/metrics/cadvisor

For solve issue with forbidden cAdvisor metrics permissions create clusterrole

```kubectl create clusterrole system:anonymous --verb=get,list,watch --resource=nodes/metrics```

```kubectl describe clusterrole system:anonymous```

```kubectl create clusterrolebinding cluster-system-anonymous --clusterrole=system:anonymous --user=system:anonymous```

```kubectl describe clusterrolebinding cluster-system-anonymous```


6) blackbox_exporter deploy

```kubectl apply -f pv-blackbox.yaml```

```kubectl create -f pv-claim-blackbox.yaml```

mkdir /mnt/blackbox and copy config.yml

```kubectl create -f blackbox-deployment-pv.yml```

 example: job_name: Webservers


