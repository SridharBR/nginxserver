# nginxserver : Deploy nginx server using kubectl

This project contains configuration files to deploy nginx server on a kubernetes cluster using kubectl. It also has roles to control access on resources to specific users.

## Create a service
Expose nginx server to listen to port "90" outside the cluster and map it to port "80" within the cluster.

```
kubectl -f apply nginx_service.yaml
```

## Create a deployment
Deploy the nginx server using the deployment configuration file

```
kubectl -f apply nginx_deployment.yaml
```
