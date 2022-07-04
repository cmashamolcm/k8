# k8

1. Container orchestration/ management system
2. From Google
3. Swarm is in-built for docker and simpler than k8s. But k8s has more features and is generic to use in any cloud platform with no change in manifest file.
4. Autoscaling is not there in swarm but is there in k8s.
5. K8 can handle security with different protocols.

# Docker

1. Containers: Instance of a docker image
2. Images: Definition of a container. Has all details to make a container run such as env variables etc.

# Local Set Up:
1. Install minikube(vm with docker in it)
2. Start minikube, point cmd to docker inside minikube and use it light weight. (minikube start --vm-driver=hyperkit)
    - If some error happens on starting existing cluster,
      - `kubectl config delete-cluster minikube`
      - `minikube delete`
      - `minikube start --vm-driver=hyperkit`
      - `kubectl get svc` lists services
      - `minikube ip` shows the ip of minikube with which we can connect to it from system

# Pods
1. Smallest unit of deployment
2. Group of containers sharing network, storage and config regarding how to run it. All containers inside a pod will have shared context
3. Mostly a pod will have only one container to manage. But rarely, there can be helper containers associated with it. Eg: a log processor service attached to a microservice.Such helper containers within the same pod are called `side car containers`
4. Pod is actually a wrapper on top of a container.

# Commands
1. To create a pod from yaml file
`kubectl apply -f first-pod.yaml` 
2. To list down all services and pods
`kubectl get all`
3. To get the details of a pod
`kubectl describe pod webapp-pod` (webapp-pod is the name of pod)
4. To execute a command inside a pod
 - `kubectl exec webapp-pod -- date`
 - `kubectl exec webapp-pod -- ls`
 - `kubectl exec webapp-pod -- pwd`
 - `kubectl exec webapp-pod -- ifconfig`
Actual command to execute should be added after `--`
 - `kubectl exec webapp-pod -- wget localhost:80` - will download index.html from fleetman web app
 - `kubectl exec webapp-pod -- cat index.html`
5. To expose pod to outside
 - `kubectl port-forward webapp-pod 30080:80` - makes the app running in 80 of pod to get exposed to 30080 of localhost. No need of minikube ip fetch now. This exposes to outside system itself.
