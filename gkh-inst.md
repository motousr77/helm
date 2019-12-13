### Next Step 

Deploy the cluster by Kind:
<!-- Create cluster from yaml definition -->
~~~sh
# - create cluster from yaml definition:
cat << EOF | kind create cluster --config -
kind: Cluster
apiVersion: kind.sigs.k8s.io/v1alpha3
nodes:
- role: control-plane
- role: worker
- role: worker
EOF
~~~
Init and update helm:
~~~sh
helm init
helm repo update
# - list helm home (locate at the $HOME/.helm):
tree $(helm home)
~~~
Then we need to configure Tiller:
~~~sh
# - create service account
kubectl create sa -n kube-system tiller
# - create cluster role binding
kubectl create clusterrolebinding tiller-cluster-role \
  --clusterrole=cluster-admin \
  --serviceaccount=kube-system:tiller
# - patch deployment
kubectl patch deploy -n kube-system tiller-deploy \
  -p '{"spec": {"template": {"spec": {"serviceAccount": "tiller"}}}}'
~~~
And now install applications...
~~~sh
# - install Prometheus-operator
export OPERA=monitoring
helm install --name operator stable/prometheus-operator
#  - forward monitoring port 9090 (remain namespace in k8s):
kubectl port-forward prometheus-operator-prometheus-operat-prometheus-0 9090 


~~~

## (Warning) Previous Content was moved into README.md
USE THIS LINK: https://github.com/motousr77/helm

###### (NEVERMIND) Other inline structures:
~~~sh
prometheus-$OPERA-prometheus-operat-prometheus-0
kubectl port-forward 
echo $(kubectl get pods | grep )
~~~