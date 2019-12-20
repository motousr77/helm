## Install Applications 

#### Install LoadBalancer
~~~sh
# ^ we will be use MetalLB
helm install --name metallb stable/metallb
~~~

#### Instead of port forwarding by kubectl (what we use in previous page) we can expose pods to the nodeports like this
~~~sh
# ^ expose Prometheus to load-balancer
kubectl expose pod $(kubectl get pods --selector app=prometheus -o jsonpath='{..metadata.name}') \
  --port=9090 --type=LoadBalancer --name=prometheus-2-lb

# ^ expose Alertm Manager to load-balancer
kubectl expose pod $(kubectl get pods --selector app=alertmanager -o jsonpath='{..metadata.name}') \
  --port=9093 --type=LoadBalancer --name=alertmanager-2-lb

# ^ expose Grafana pod to load-balancer
kubectl expose pod $(kubectl get pods --selector app=grafana -o jsonpath='{..metadata.name}') \
  --port=3000 --type=LoadBalancer --name=grafana-2-lb
~~~

#### Install Ingress
