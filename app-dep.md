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
  --port=9090 --type=LoadBalancer --name=prometheus-2-lb --labels='exp=lb'

# ^ expose Alertm Manager to load-balancer
kubectl expose pod $(kubectl get pods --selector app=alertmanager -o jsonpath='{..metadata.name}') \
  --port=9093 --type=LoadBalancer --name=alertmanager-2-lb --labels='exp=lb'

# ^ expose Grafana pod to load-balancer
kubectl expose pod $(kubectl get pods --selector app=grafana -o jsonpath='{..metadata.name}') \
  --port=3000 --type=LoadBalancer --name=grafana-2-lb --labels='exp=lb'
~~~

#### Get node ip addresses
~~~sh
kubectl get nodes -o jsonpath='{..status.addresses[].address}'
~~~

#### Label services
~~~sh
# ^ do this if didn't it in previous steps 
kubectl label service/alertmanaget-2-lb exp=lb
kubectl label service/prometheus-2-lb exp=lb
kubectl label service/grafana-2-lb exp=lb
~~~

And then we can print our services and ports
~~~sh
kubectl get svc --selector exp=lb

# ^ and print ip addresses of the Nodes
kubectl get nodes -o jsonpath='{..status.addresses[0].address}'
#  or 
kubectl get nodes -o jsonpath='{..status..address}'
# and save some data (some of IP)
export SOME_IP=$(kubectl get nodes -o jsonpath='{..status..address}')

# ^ if work below done and load-balancer working correctly
#   we can access to application by connect to any nodes like this
curl $SOME_IP:<application-port>
~~~

... to be continued ...

#### Install Ingress
~~~sh
# ^ ...
helm install --name ingress-ng stable/nginx-ingress
~~~

#### Instal Kubernetes Dashboard
~~~sh
# ^ install by helm
helm install --name kub-dashboard stable/kubernetes-dashboard

# ^ check installation pod
kubectl get pods --selector app=kubernetes-dashboard

# ^ and expose to nodeport
kubectl expose pod $(kubectl get pods --selector app=kubernetes-dashboard -o jsonpath='{..metadata.name}') \
  --port=8443 --type=LoadBalancer --name=kub-dashboard-2-lb

~~~