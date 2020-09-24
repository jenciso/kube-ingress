## HAPROXY LOADBALANCER 

Your need to have a haproxy loadbalancer (or similar). If you have a haproxy, in order to work with this nginx controller, your config file `/etc/haproxy/haproxy.cfg` will should these entries


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
