# Kubernetes-wordpress-fpm-nginx
Kubernetes custom wordpress deployment with php-fpm & nginx ingress


## Getting Started

These instructions will get you a copy of the project up and running on your cloud machine for development and testing purposes.


### Prerequisites

Kubernetes cluster with nginx ingress installed

```
Kubernetes <= 0.10

```

### Installing

A step by step series of examples that tell you how to get a setup wordpress env running


Build custom WordPress docker image using official php-fpm WordPress docker version

Official WordPress docker : https://hub.docker.com/_/wordpress

Exmple docker file

```
FROM wordpress:5-php7.2
COPY ./ /usr/src/wordpress/
EXPOSE 80
EXPOSE 9000
```
### If WordPress taking Db connection values from envrionment variable

Edit & apply wordpress-configmap.yaml

```
kubectl apply -f wordpress-configmap.yaml
```

### If want to implment fast-cgi with nginx ingress

Edit & apply fast-cgi-configmap.yaml & ingress.yaml (uncomment annotations)

```
kubectl apply -f fast-cgi-configmap.yaml

kubectl apply -f ingress.yaml
```

### SSL/TLS certificate

If want to use SSL/TLS cetificate setup cert-manager and apply clusterissuer

Edit & apply cluster-issuer.yaml

```
kubectl apply -f cluster-issuer.yaml
```

### WordPress service deployment

WordPress deployment will create One with Two container inside it (Nginx + Wordpress php-fpm)

Edit & apply wordpress-deployment.yaml & wordpress-service.yaml

```
kubectl apply -f wordpress-service.yaml

kubectl apply -f wordpress-deployment.yaml
```

### Traffic flow

nginx ingress > nginx container inside POD > wordpress container (over localhost in same POD)