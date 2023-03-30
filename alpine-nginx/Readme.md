# Docker File for Nginx1.19 on alpine

This Dockerfile is a starting point for running a secure Nginx server with HTTPS support in a Docker container.

alipine:3.17 AS base
nginx:1.19-alpine

# BUILD
to build the Docker File 
```bash
.
 docker build . -t alpine-nginx
 docker tag alpine-nginx itsvamc02/alpine-nginx:latest
 docker push itsvamc02/alpine-nginx:latest
```

# RUN 
To run the image built above

```bash
docker run -dp 8080:8080 alpine-nginx
```
Access
https://example.com/
OR
https://localhost:8080

you should see nginx started and running as nginx user (not root )

```bash
❯ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS         PORTS                                     NAMES
3c8275a3870b   alpine-nginx   "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds   80/tcp, 443/tcp, 0.0.0.0:8080->8080/tcp   goofy_mcclintock
❯ docker exec -it 3c8275a3870b /bin/sh
/ $ ps -ef
PID   USER     TIME  COMMAND
    1 nginx     0:00 nginx: master process nginx -g daemon off;
   23 nginx     0:00 nginx: worker process
   24 nginx     0:00 nginx: worker process
   25 nginx     0:00 nginx: worker process
   26 nginx     0:00 nginx: worker process
   27 nginx     0:00 nginx: worker process
   28 nginx     0:00 nginx: worker process
```


# Notes 
1. Pull the latest alpine image as base.
2. installs the necessary packages for Nginx and openssl
3. creates required ssl directory and installs self-signed SSL/TLS certificate
4. uses a multi-stage build to reduce the image size.
5. copies custom nginx.conf file to /etc/nginx.conf
6. pull the official docker nginx:1.19-alpine
7. copies ssl certs & nginx from previous stages (3 & 4 steps) in build 
8. sets secure file permissions on the certificate and key (/etc/ssl/private)
9. re-create files and directories in layer2 required for nginx user and change permssion 
9. switch to Nginx user & exposes ports 8080 and 443 for HTTP and HTTPS traffic
10. starts Nginx  
