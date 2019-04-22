#  Ingress Controller  Nginx

## Ingress Nginx
Version： `nginx-0.24.1`
https://github.com/kubernetes/ingress-nginx/blob/nginx-0.24.1/deploy/mandatory.yaml
https://github.com/kubernetes/ingress-nginx/blob/nginx-0.24.1/deploy/provider/baremetal/service-nodeport.yaml

## Haproxy
配置: `/etc/haproxy/haproxy.cfg`
```bash
frontend http_80_in
       bind 0.0.0.0:80
       mode http
       option forwardfor
       option httplog
       option httpclose
       option dontlognull
       log 127.0.0.1 local3

       acl DEMO_JAVA hdr_dom(host) -i demo.linuxhub.cn
       use_backend demo_java if  DEMO_JAVA

backend demo_java
        mode http
        option forwardfor
        balance roundrobin
        timeout server    60s
        server k8s-node-12 192.168.8.12:60080 check inter 2000 fall 2 rise 2 weight 1
        server k8s-node-13 192.168.8.13:60080 check inter 2000 fall 2 rise 2 weight 1
        server k8s-node-14 192.168.8.14:60080 check inter 2000 fall 2 rise 2 weight 1
```

## 访问
demo.linuxhub.cn

