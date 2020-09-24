## Ingress Controller


Create a dhparam certificate:

```shell
openssl dhparam -out ssl-certs/dhparam.pem 4096
```

Install the wildcard SSL and dhparam certificate as secrets

```shell
kubectl create secret generic tls-dhparam --from-file=ssl-cert/dhparam.pem -n ingress-nginx
kubectl create secret tls  tls-wildcard--paas --key ssl-cert/paas-lab.docs-planet.site.key --cert ssl-cert/paas-lab.docs-planet.site.key -n ingress-nginx
```

Apply the yaml files to create a internal ingress

```shell
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/default-backend.yaml
kubectl apply -f k8s/rbac.yaml
kubectl apply -f k8s/service-nodeport.yaml
kubectl apply -f k8s/with-rbac-ssl-default.yaml
```

To create a external ingress, apply these files

```shell
kubectl apply -f k8s/configmap_external.yaml
kubectl apply -f k8s/service-nodeport_external.yaml
kubectl apply -f k8s/with-rbac-ssl-default_external.yaml
```
