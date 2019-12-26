## Install Applications in the Kind cluster

#### Install Load-Balancer
~~~sh
# ^ we will be use MetalLB
helm install --name metallb stable/metallb
~~~

#### Instead of port forwarding by kubectl we can expose pods to the nodeport by load-balancer
~~~sh
# ^ expose Prometheus to load-balancer
kubectl expose pod $(kubectl get pods --selector app=prometheus -o jsonpath='{..metadata.name}') \
  --port=9090 --type=LoadBalancer --name=prometheus-2-lb --labels='exp=lb'

# ^ expose Alertmanager to load-balancer
kubectl expose pod $(kubectl get pods --selector app=alertmanager -o jsonpath='{..metadata.name}') \
  --port=9093 --type=LoadBalancer --name=alertmanager-2-lb --labels='exp=lb'

# ^ expose Grafana pod to load-balancer
kubectl expose pod $(kubectl get pods --selector app=grafana -o jsonpath='{..metadata.name}') \
  --port=3000 --type=LoadBalancer --name=grafana-2-lb --labels='exp=lb'
~~~

#### Label services and test connections
~~~sh
# ^ do this if didn't it in previous steps 
# === SOME CASE ===
kubectl label service/alertmanager-2-lb exp=lb
kubectl label service/prometheus-2-lb exp=lb
kubectl label service/grafana-2-lb exp=lb
# === SOME CASE ===

# ^ And now we can print our services and ports
kubectl get svc --selector exp=lb

# and export node IP address
export SOME_IP=$(kubectl get nodes -o jsonpath='{..status..address}' | cut -d ' ' -f 1)

# ^ if steps below done and load-balancer working correctly
#   then we can access to application throughout connect to any nodes like this
# === DEFINITION === > curl $SOME_IP:<application-port>
curl $SOME_IP:31777

# ^ this scope will be automated further (we can export application service node port)
~~~

### To be continued . . .

#### Install Kubernetes Dashboard (will be redacted)
=== Here some bugs with deployment and need to test ===
~~~sh
# ^ create cluster-role-binding for the K8s Dashboard
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

# ^ install the K8s Dashboard by Helm
helm install --name dashboard stable/kubernetes-dashboard --namespace kube-system --set rbac.create=True --set rbac.clusterReadOnlyRole=True

# ^ expose pod of the K8s Dashboard to the node-port (load-balancer)
kubectl expose pod \
  $(kubectl -n kube-system get pods --selector app=kubernetes-dashboard -o jsonpath='{..metadata.name}') \
  --port=8443 --type=LoadBalancer --name=kub-dashboard-2-lb --labels='exp=lb'

~~~
