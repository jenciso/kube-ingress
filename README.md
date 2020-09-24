## Ingress Controller


Create a dhparam certificate:

```shell
openssl dhparam -out ssl-certs/dhparam.pem 4096
```

Install a wildcard SSL certificate as a secret

```shell
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

In your loadbalancer you need to have a haproxy server (or similar). If you have a haproxy, your config file `/etc/haproxy/haproxy.cfg` it should has these entries


```conf
## nginx-controller-internal

frontend nginx-controller-internal-80
    bind        10.64.13.233:80
    mode tcp
    default_backend nginx-controller-internal-backend-80

backend nginx-controller-internal-backend-80
    balance roundrobin
    mode tcp
    server infra01 10.64.13.217:30080 check
    server infra02 10.64.13.218:30080 check
    server infra03 10.64.13.219:30080 check


frontend nginx-controller-internal-443
    bind        10.64.13.233:443
    mode tcp
    default_backend nginx-controller-internal-backend-443

backend nginx-controller-internal-backend-443
    balance roundrobin
    mode tcp
    server infra01 10.64.13.217:30443 check
    server infra02 10.64.13.218:30443 check
    server infra03 10.64.13.219:30443 check
```

```conf
## nginx-controller-external

frontend nginx-controller-external-80
    bind        10.64.13.231:80
    mode tcp
    default_backend nginx-controller-external-backend-80

backend nginx-controller-external-backend-80
    balance roundrobin
    mode tcp
    server infra01 10.64.13.217:31080 check
    server infra02 10.64.13.218:31080 check
    server infra03 10.64.13.219:31080 check


frontend nginx-controller-external-443
    bind        10.64.13.231:443
    mode tcp
    default_backend nginx-controller-external-backend-443

backend nginx-controller-external-backend-443
    balance roundrobin
    mode tcp
    server infra01 10.64.13.217:31443 check
    server infra02 10.64.13.218:31443 check
    server infra03 10.64.13.219:31443 check
```
