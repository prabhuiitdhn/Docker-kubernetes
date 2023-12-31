# Kubernetes

__Kubernetes:__

- It is used for managing muliple number of containers, a container orchestraon.
- It is a open source container orchestrain tool, developed by google, which is used for managing containerized applications in different environment.
- It actually offered the perfect host for each independent containers

Features do kubernetes:
1. High availability or no downtime
1. scalability or high performace
1. disater recovery - backup and restore


## Architecture.

It has a master node and respective required worker[child] node. where each child can communicate to each other.

kubelet: It is a process of communicating each worker node to each other where each worker node will have docker container for apllication deployed on it Depending on work load distributed on each worker node, It will have container, and master node, will run conatiner which are neccessary to run and manage to run it.

__Master node will have following container:__

*1. Entry point of kubernetes cluster is API sever.*

*2. Controller manager: which keeps track of what is happening in the cluster. which keeps track of when to start which container.*

*3. scheduler: scheduling conatiner on different node based on workload, and server requirement. based on the requirement and availability of conatiner it schedules and run it.*

*4. etcd: key value store and hold the current state of container. using snapshot*

*5. virtual network: where worker node and master node talks to each other. It should be a powerful machine  which will have resources to all mode, it will have more resources and much bigger.*

*master node is important, If we loose it and we will not have keep tracks of any other node.*

__Kubernets components:__

- __Node and Pod__: smallest unit of k8s, abstraction over container. pod actually create a running environment over container, using pod we will only be interacting with kubernets layer
                Pod is meant for running one application per pod
                to communicating with each other, each pod will have its own IP address
                any IP address/node can die easily bcz of interruption and will assign new ip address & managing the communication becomes harder so, for this issue another component comes is service

- __Service__: It is the components when any node/ip address dies by any interruption.
            So, it basically set a permanent IP address for each pod, and lifecycle of pod and service and not connected. i.e if ip address dies any cause, it will still maintain the same IP address
             & application will be accessed with browser. and specify internal service and external service., External service is for which can publicly available and internal is for internal purpose.
             and in this case http protocol will be with ip address with port which is not good for deployment so , change the url like domain name, ingress component works

- __Ingress__: It folds the ip address with port with domain name

    so, pod communicate with each other using service.
    so assume, one service is mongoDB and all but where it is being configured.

- __ConfigMap__: external configuration of application; using this we do not have to be rebuld the docker again. just need to map the service with database [in example]
                           and mongoDB will need to have username and pass word, and kubernetes has 'secert'
-__'secret'__ - which only keeps the authentication things, like password, username and signature or anything related to authentication

- __Volume__: it is used for Data storage. so, assume if pod is restarted then all the data which is being kept in node will be gone so,
            kubenetes has component named as 'volume' which will keep data for persistent for ling term,
            so it basically attaches physical storage memory in harddrive and it could be in local and remote store. which is not part of kubenetes.kubernetes does not manage any data persistence.

- __Deployment stateful set__:
        in the state of deployement, If one node dies then It requests to another node as the same time which will have a replica of deleted node
        in this case, If server stopped working then it can still request another node to perform the deployment
        It happens same for databases It will also have replica which are linked with same data storage, which maintains the data inconsistency. which handled by another component called as "Stateful set"

- __Stateful set__: It is for stateful Apps/Databases which has to be run continuous.

__Summary:__
we have set of core components in Kubernetes:
```
1. Pod: node which will have docker
2. Service: It is for managing the muliple pods
3. ingress: It is used for mapping ip address to domain
4. Volumes: Storage for databases
5. ConfigMap: Mapping the databases to pod/service
6. Secrets: stores Secret authentications key for databases
7. Deployment: It is stateless set
8. Stateful set: Making deployment stateful.
   ```

__Minikube:__
- It is open source tool for testing on local environment and helps to deployment after setting up the clusters.
- It is one node cluster, will have master and worker process in same node.
- It creates a virtual box in local system and runs nodel in virtual box for testing purpose.

__Kubectl:__

After setting up the cluster, Cubctl helps to communicate, configured and manged with clusters, and It is command line tool for Kubernetes.
minukube works with master and worker so, one master processes/node will be API server which is entry point for the cluster and enables interaction with cluster.

+ creates pod, delete the pod and create service.
+ It can also work with cloud cluster or anykind of kubernetes cluster.

Minikube needs virtualization on the machine.

Download the minikube:

[https://minikube.sigs.k8s.io/docs/start/]

command:

    minikube start # It actually start the minikube based any virtual box, In my case, docker.
                    # It will start taking the minikube docker image into the docker.
                    
                    output:  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
    
    kubectl get nodes # It will return the node ruuning.
    
        NAME       STATUS   ROLES           AGE    VERSION
        minikube   Ready    control-plane   116s   v1.27.4

    minikube status # will show if minikube is running successfully.t
    
        Output:
        minikube
        type: Control Plane
        host: Running
        kubelet: Running
        apiserver: Running
        kubeconfig: Configured

    kubectl version #will show the version of it.

## create components

    kubectl get pod # it find the pod running
    kubectl get services # it finds the service running
        Output:
        C:\Users\uib43225>kubectl get services
        NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
        kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5m46s

    kubectl create --help # used for getting help for creating component
        pod is smallest unit so, we are not creating it, it will create as abtsraction layer or deployment.

    kubectl create deployment NAME --image =image --dry_run
    kubectl create deployment nginx-depl --image=nginx # it will create deployment with base image nginx
    kubectl get deployment # it will return the deployment if it creates successfully
        NAME         READY   UP-TO-DATE   AVAILABLE   AGE
        nginx-depl   0/1     1            0           43s
    
    kubectl get pod # now it will return the pod which is inside the deployment.
        Output:
        C:\Users\uib43225>kubectl get pod
        NAME                          READY   STATUS             RESTARTS   AGE
        nginx-depl-6b7698588c-8w67g   0/1     ImagePullBackOff   0          2m12s
    
    kubectl logs nginx-depl-6b7698588c-8w67g # which basically show what are the pods running and can be logged
    kubectl describe pod_name # will describe the deployment or pod name i.e. shows which image is being taken and all
    kubectl exec -it pod_name -- bin/bash # will give the terminal of running pod
    kubectl get replicaset # will return the replica of deployment which is ready for the configured.
    kubectl apply -f config_file.yaml # It will execute whatever in the confiuration file
        - in the configuration file, we can configuration number of containers, which image, which deployment and so many in configuration
        - we can alos change the number of replica we want to have
    
    kubectl get all # will shows all the pod, services running
    
    minikube service service_name # open the url/ip address in web browser.

__Example with MongoDB and mongo-Express.__

 - First we will create mongoDb pod
 - Create internal service for MongoDB
 - Create mongoDB express deployment
 - Authentication through env variable.
 - Create config map which contains the data base url
 - Create secret which keeps the authentication
 - Referencing inside the deployment file
 - Once we have setup, need mongo express for external request which can talk to pod/url.
   
Pipeline: webbroweser/url -> MongExpress(External service) -> MongoExpress(pod) -> MongoDB Internal service -> MongoDB

Example can be found in this timestamp: [https://www.youtube.com/watch?v=Wf2eSG3owoA&t=13542s] : 3:45:42 timestamp
