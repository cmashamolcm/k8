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

# Service
1. Pods has short life. It gets killed, restarted etc. So, there should be a layer that is kind of stable. That is a service.
2. Service has stable IP and ports.
3. Service is under the category of `load balancer or discovery`
4. Service is a gateway to connect to the pods in a k8 cluster. It helps pods to interact with each other.
5. `Labels` are series of key value pairs added in Pods
6. `Selectors` are series of key value pairs added in Service
7. A service can detect pods with same label as selector within a cluster
8. `Selector` comes under spec part of service yaml.
9. `Label` comes under metadata part of pod yaml
10. Type:
    - clusterID - private to cluster
    - NodePort - to expose to outside world
11. When exposes port via nodePort in service, it is not the port forwarding. So, need to use minikube ip and then service node port to access service outside.

## ActiveMQ Pod and Service Setup:
1. Using the image of ActiveMQ and build pod and make a service.

## Replicaset:
1. `kubectl delete pod pod-name` - deletes pod gracefully.
2. APIs for replicaset is part of "apps" whereas pod and service apis are part of "core" which is default. So, whiledefining replicaset, we have to specify apiVersion as `apps/v1`
3. delete:
    - `kubectl delete rs --all` - deletes replicaset and associated pods
    - `kubectl delete po --all`
    - `kubectl delete svc --all`
5. Replicaset helps to create multiple copies of pods with a `template` to maintain continuous availability of service.
6. By default one replica gets created even if we are not specifying the `replicas` count in replicaSet yaml under spec.
7. Whenever the pod goes down, a new one gets created.
8. If we specify 2 as replicas, everytime k8s will maintain 2 pods up and running.
9. ```
    In replicaSet under spec, seletor is mandatory.
    In selector, there is matchLabels tag. This should be matching to the labels given in spec under template(pod definition within replicaset)
    ```
10.
