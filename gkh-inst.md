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

Use/Check contexts in kubernetes cluster
~~~sh
# ^ (Optional) get contexts list
kubectl config get-contexts
~~~

Optional - because switch to another context need we have several kind-clusters

~~~sh
# ^ (Optional) switch to kind context
kubectl config use-context kind-kind
~~~

Also we can labeled worker nodes like this
~~~sh
# ^ very simple but i want to improve it (automate for several nodes)
kubectl label node/<worker-name> node-role.kubernetes.io/worker=
~~~

Initialize and Update the Helm reposiroty
~~~sh
# ^ _
helm init && helm repo update

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

And now install any applications _ goto Next page
~~~sh
# ^ install Prometheus-operator
helm install --name monitoring stable/prometheus-operator
~~~

Forward some services
~~~sh
# ^ forward prometheus operator port
kubectl port-forward $(kubectl get pods --selector app=prometheus -o jsonpath='{..metadata.name}') 9090

# ^ forward alert manager port
kubectl port-forward $(kubectl get pods --selector app=alertmanager -o jsonpath='{..metadata.name}') 9093

# ^ forward grafana port
kubectl port-forward $(kubectl get pods --selector app=grafana -o jsonpath='{..metadata.name}') 3000
~~~

#### Next Page [Application Deployments](https://github.com/motousr77/helm/blob/master/app-dep.md)