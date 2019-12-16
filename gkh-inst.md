### Create Kind-cluster & Install Applications by Helm

Deploy the cluster by Kind
~~~sh
# ^ create cluster from yaml definition:
cat << EOF | kind create cluster --config -
kind: Cluster
apiVersion: kind.sigs.k8s.io/v1alpha3
nodes:
- role: control-plane
- role: worker
- role: worker
EOF
~~~

Initialize and Update the Helm reposiroty
~~~sh
helm init
helm repo update

# ^ list Helm home (locate at the $HOME/.helm):
tree $(helm home)
~~~

Then need to configure Tiller in the cluster
~~~sh
# ^ create service account
kubectl create sa -n kube-system tiller

# ^ create cluster role binding
kubectl create clusterrolebinding tiller-cluster-role \
  --clusterrole=cluster-admin \
  --serviceaccount=kube-system:tiller

# ^ patch deployment
kubectl patch deploy -n kube-system tiller-deploy \
  -p '{"spec": {"template": {"spec": {"serviceAccount": "tiller"}}}}'
~~~

And now install any applications...
~~~sh
# ^ install Prometheus-operator
export OP_NAME=monitoring
helm install --name $OP_NAME stable/prometheus-operator

# ^ forward monitoring port 9090 (remain namespace in k8s)
kubectl port-forward $(kubectl get pods --selector prometheus -o jsonpath='{.items[*].metadata.name}') 9090

# ^ forward prometheus operator port
kubectl port-forward $(kubectl get pods --selector app=prometheus -o jsonpath='{.items[*].metadata.name}') 9090

# ^ forward alert manager port
kubectl port-forward $(kubectl get pods --selector app=alertmanager -o jsonpath='{.items[*].metadata.name}') 9093

# ^ forward grafana port
kubectl port-forward $(kubectl get pods --selector app=grafana -o jsonpath='{.items[*].metadata.name}') 3000
~~~

Templates for some util operations:
~~~sh
kubectl patch svc \
  $(kubectl get svc --selector app=grafana \
    -o jsonpath='{.items[*].metadata.name}') \
    --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'
~~~

### To be continued!
In the next solution i try to describe other useful tools and applications for Kubernetes cluster.