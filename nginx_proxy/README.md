# /nginx_proxy

## Files

```
./
│   Dockerfile
│
└───proxyfiles
    │   reverse-proxy.conf
```
### [/nginx_proxy/proxyfiles/reverse-proxy.conf](/nginx_proxy/proxyfiles/reverse-proxy.conf)
In this file is were there are declared the behavior of the proxy. If the url begins with /flask/ the request will be redirect to the pythonapp in the port 5000. If the url begins with /nginx/ the request will be redirect to the nginx static files server.

```
server {
  listen 80;
  listen [::]:80;

  server_name 54.202.8.127;

  location /flask/ {
      proxy_pass http://pythonapp:5000/;
  }

  location /nginx/ {
      proxy_pass http://nginxstatic:8080/;
  }
}
```

### [/nginx_proxy/Dockerfile](/nginx_proxy/Dockerfile)
This file were used to build a new image based on a nginx image from docker hub. Once the image were downloaded, copy the file [/nginx_proxy/proxyfiles/reverse-proxy.conf](/nginx_proxy/proxyfiles/reverse-proxy.conf) in the folder /etc/nginx/conf.d/ inside of the container in order that the proxy works.

```
FROM nginx
COPY ./proxyfiles /etc/nginx/conf.d/
```