# Ingress Controller

Create a namespace
```shell
kubectl create ns ingress-nginx
```

Create a dhparam certificate:
```shell
openssl dhparam -out ssl-certs/dhparam.pem 1024
kubectl create secret generic tls-dhparam --from-file=ssl-certs/dhparam.pem -n ingress-nginx
```

Additional config
```shell
kubectl apply -f k8s/default-backend.yaml
kubectl apply -f k8s/rbac.yaml
```

Create a certificate

```
sudo certbot certonly   --dns-cloudflare   --dns-cloudflare-credentials /root/.secrets/cloudflare.ini   -d paas.docs-planet.site,*.paas.docs-planet.site   --preferred-challenges dns-01
```

## Internal Ingress

```shell
kubectl create secret tls  tls-wildcard--paas --key ssl-certs/paas.docs-planet.site.key --cert ssl-certs/paas.docs-planet.site.crt -n ingress-nginx
```

```shell
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/service-nodeport.yaml
kubectl apply -f k8s/with-rbac-ssl-default.yaml
```

## External ingress


```shell
kubectl create secret tls  tls-wildcard--docs-planet --key ssl-certs/docs-planet.site.key --cert ssl-certs/docs-planet.site.crt -n ingress-nginx
```

```shell
kubectl apply -f k8s/configmap_external.yaml
kubectl apply -f k8s/service-nodeport_external.yaml
kubectl apply -f k8s/with-rbac-ssl-default_external.yaml
```

## HAPROXY setup

Read [HAPROXY.md](./HAPROXY.md)

## Testing

```
kubectl create deployment nginx-demo --image=nginx
kubectl expose deployment nginx-demo --port 80 --name=nginx-demo
kubectl apply -f ingress-nginx-demo.yml
```
