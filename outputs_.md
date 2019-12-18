~~~sh
helm install --name ng-ing stable/nginx-ingress --namespace kube-system --set controller.service.loadBalancerIP="10.0.2.15"
~~~

NAME:   ng-ing
LAST DEPLOYED: Mon Dec 16 12:14:50 2019
NAMESPACE: kube-system
STATUS: DEPLOYED

RESOURCES:
==> v1/Deployment
NAME                                  AGE
ng-ing-nginx-ingress-controller       1s
ng-ing-nginx-ingress-default-backend  1s

==> v1/Pod(related)
NAME                                                   AGE
ng-ing-nginx-ingress-controller-7b569bdc8b-gj9j9       1s
ng-ing-nginx-ingress-default-backend-7b44f7d968-6ql9m  1s

==> v1/Service
NAME                                  AGE
ng-ing-nginx-ingress-controller       1s
ng-ing-nginx-ingress-default-backend  1s

==> v1/ServiceAccount
NAME                          AGE
ng-ing-nginx-ingress          1s
ng-ing-nginx-ingress-backend  1s

==> v1beta1/ClusterRole
NAME                  AGE
ng-ing-nginx-ingress  1s

==> v1beta1/ClusterRoleBinding
NAME                  AGE
ng-ing-nginx-ingress  1s

==> v1beta1/Role
NAME                  AGE
ng-ing-nginx-ingress  1s

==> v1beta1/RoleBinding
NAME                  AGE
ng-ing-nginx-ingress  1s


NOTES:
The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace kube-system get services -o wide -w ng-ing-nginx-ingress-controller'

An example Ingress that makes use of the controller:
~~~yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: example
  namespace: foo
spec:
  rules:
    - host: www.example.com
      http:
        paths:
          - backend:
              serviceName: exampleService
              servicePort: 80
            path: /
  # This section is only required if TLS is to be enabled for the Ingress
  tls:
      - hosts:
          - www.example.com
        secretName: example-tls
~~~

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:
~~~yml
apiVersion: v1
kind: Secret
metadata:
  name: example-tls
  namespace: foo
data:
  tls.crt: <base64 encoded cert>
  tls.key: <base64 encoded key>
type: kubernetes.io/tls
~~~


create custom Ingress
~~~sh
cat <<EOF | kubectl apply -f -
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: grafana-ing
  namespace: default
spec:
  rules:
    - host: localhost
      http:
        paths:
          - backend:
              serviceName: monitoring-grafana
              servicePort: 80
            path: /grafana
EOF
~~~
<!-- kubectl expose deployment hello-world --type=LoadBalancer --name=my-service -->

kubectl expose deployment monitoring-grafana --type LoadBalancer --name=grafana-svc
~~~sh
# get internal IP of all nodes
kubectl get nodes -o jsonpath='{.items[*].status.addresses[0].address}'
~~~
