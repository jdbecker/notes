# Notes
This repository contains my personal notes and how-tos. You're welcome, future
me.

## Setup
To get set up to use and edit this repository locally, the best way is to
install [VSCode](https://code.visualstudio.com/Download), with the [github 
plugin](<vscode:extension/GitHub.vscode-pull-request-github>).

## Kubernetes Notes
- `minikube version` : verifies minikube is installed (minikube is a small,
local kubernetes cluster with only a single node, useful for prototyping)
- `minikube start` : starts minikube cluster
- `kubectl version` : verifies kubectl is installed (kubectl is a cli for
interfacing with kubernetes)
- `kubectl cluster-info` : displays cluster details
- `kubectl get nodes` : displays nodes
- `kubectl create deployment [name] --image=[image]` : adds a "deployment" to
the kubernetes cluster, which monitors instances of the application on the nodes
and deploys the application when fewer than expected are running.
- `kubectl get deployments` : list deployments currently configured in the
cluster
- `kubectl get pods` : as above, only lists pods (you see how this works)
- `kubectl describe pods` : a very wordy list of pod details
- `kubectl proxy` : starts a service that, while running, shares all the 
services running in the cluster with localhost.
- `kubectl logs $POD_NAME` : returns anything that the managed applications
would normally send to STDOUT.
- `-l [key=value]` : can add this option to commands like `get pods` and 
`get services` to filter by label.

## Snippets
### Get pod name
```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```
### Get pod output
```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```
### List container env variables
```bash
kubectl exec $POD_NAME -- env
```
### Start bash session in pod's container
```bash
kubectl exec -ti $POD_NAME -- bash
```

## Glossary
### Control Plane
> The Control Plane is responsible for managing the cluster. The Control Plane 
coordinates all activities in your cluster, such as scheduling applications, 
maintaining applications' desired state, scaling applications, and rolling out 
new updates.
### Deployment
> A Deployment is responsible for creating and updating instances of your 
application. It monitors the number of currently running instances of your
application and deploys more when fewer than expected are running.
### Node
> A node is a VM or a physical computer that serves as a worker machine in a 
Kubernetes cluster. Each node has a Kubelet, which is an agent for managing the 
node and communicating with the Kubernetes control plane. The node should also 
have tools for handling container operations, such as containerd or Docker.
### Pod
> A Pod is a group of one or more application containers (such as Docker) and 
includes shared storage (volumes), IP address and information about how to run 
them.
### Service
> A Kubernetes Service is an abstraction layer which defines a logical set of 
Pods and enables external traffic exposure, load balancing and service discovery 
for those Pods.
