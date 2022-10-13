# nginxserver : Deploy nginx server on a kubernetes cluster

This project contains steps to configure a local kubernetes cluster, steps and configuration files to deploy nginx server on the kubernetes cluster.
It also has roles to control access on kubernetes resources to specific users.

## Create a local cluster using kubeadm
Below are the essential steps to setup a simple kubernetes cluster

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

### Master isolation
```
# To run pods on master node in a single node kubernetes cluster.
kubectl taint nodes --all node-role.kubernetes.io/master-
```

### Join the nodes
```
# Run the below command on all the nodes which needs to be included in cluster.
# Use the command from output of kubeadm init
kubeadm join --token <token> <master-ip>:<master-port>
```

## Deploy Nginx server on the kubernetes cluster

### Create roles
Define the roles that are permitted on the namespace in which the nginx will be deployed

```
kubectl create -f nginx_roles.yaml
```

### Create role bindings 
Bind the roles to specific users on a particular namespace

```
kubectl create -f nginx_rolebindings.yaml
```

### Create a service
Expose nginx server to listen to port "90" outside the cluster and map it to port "80" within the cluster.

```
kubectl apply -f nginx_service.yaml
```

### Create a deployment
Deploy the nginx server using the deployment configuration file

```
kubectl apply -f nginx_deployment.yaml
```
