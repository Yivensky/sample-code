# HTTPD LOAD BALANCER AND DOCKER
## Installation of httpd and docker
```
yum -y update
yum install -y httpd docker
```
## Configuration of httpd
```
rm -f /etc/httpd/conf.d/welcome.conf
cat > /etc/httpd/conf.d/lb.conf <<EOF
ProxyRequests off
<Proxy balancer://mycluster>
    BalancerMember http://127.0.0.1:8080
    BalancerMember http://127.0.0.1:8081
    BalancerMember http://127.0.0.1:8082
    BalancerMember http://127.0.0.1:8083
    BalancerMember http://127.0.0.1:8084
    BalancerMember http://127.0.0.1:8085
    ProxySet lbmethod=byrequests
</Proxy>
ProxyPass / balancer://mycluster/
EOF
```
## Start docker and httpd
```
systemctl enable docker
systemctl start docker
systemctl enable httpd
systemctl start httpd
```

eval $(aws ecr get-login --region us-east-1 --no-include-email)
image=453500636975.dkr.ecr.us-east-1.amazonaws.com/mario:latest

docker pull $image
systemctl enable httpd
systemctl start httpd
## bootsrap script
```
#!/bin/bash
docker run -p 8080:80 xx:v1 &
docker run -p 8081:80 xx:v1 &
docker run -p 8082:80 xx:v1 &
docker run -p 8083:80 xx:v1 &
docker run -p 8084:80 xx:v1 &
docker run -p 8085:80 xx:v1 &
```

## Creation of the Dockerfile
```
FROM amazonlinux:latest
EXPOSE 80

RUN yum -y install wget
RUN wget xxxxx
RUN chmod +x server
CMD ./server

docker build -t xxxxxxxxx:v1 .
```
```
FROM amazonlinux:latest
EXPOSE 80

RUN yum -y install python3 python3-pip git wget
WORKDIR /
COPY ./* /api/
WORKDIR /api
RUN pip3 install -r requirements.txt
CMD python3 main.py
```
