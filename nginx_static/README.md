# /nginx_static

## Files

```
./
│   Dockerfile
|
└───page
    │   index.html
```
### [/nginx_static/page/index.html](/nginx_static/page/index.html)
This file is a simple html page used as an example of static content.

```
<html>
    <title> Static server </title>
    <body>
        Docker static server 
        
    </body>
</html>
```

### [/nginx_static/Dockerfile](/nginx_static/Dockerfile)
This file were used to build a new image based on a nginx image from docker hub. Once the image were downloaded, copy the file [/nginx_static/page/index.html](/nginx_static/page/index.html) in the folder /usr/share/nginx/html inside of the container in order that the user can access to the static content.

```
FROM nginx
COPY ./page /usr/share/nginx/html
```
