# nginxserver : Deploy nginx server using kubectl

This project contains configuration files to deploy nginx server on a kubernetes cluster using kubectl. It also has roles to control access on resources to specific users.

## Create a local cluster using kubeadm
Below are some of the basic and essential steps to setup a simple kubernetes cluster

### Initialise control plane
```
# As we will be using Calico for pod-network, pass the respective parameter to init
kubeadm init --pod-network-cidr=192.168.0.0/16
```

### Install a pod-network
```
# Calico is our pod-network provider
kubectl apply -f http://docs.projectcalico.org/v2.3/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml
```

### Join the nodes
```
# Run the below command on all the nodes which needs to be included in cluster.
# Use the command from output of kubeadm init
kubeadm join --token <token> <master-ip>:<master-port>
```

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
