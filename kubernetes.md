# Kubernetes Architecture

![kubernetes_architecture_diagram.png](attachment:kubernetes_architecture_diagram.png)


* kubectl: calls the high-level api 
* kube-scheduler: makes sure the workloads that you've defined are always running and available whitin inside your cluster
* ectd: key-value database that keeps all the configuration in check and manages all that within kubernets
* kube-controller-manager: responsible for managing all the aspects of the kubernets cluster

* whithin each node:
    * kubelet: comunicating back to the api and making sure everything is running fine on each node
    * k-proxy: managing proxies so that the user always reaches what he wants (looks for the appropriate workload)
    * both above are managed by iptables on linux

    * container runtime: providing the capabilities to run what you want on linux

    * pods (containing the work-loads)

## Namespaces

* Provides a mechanism for isolating resources within a single cluster
* assignment of resource quotas to individual namespaces so that individual groups or user do not use too much of the resources outside of what is designated for them.
* Each namespace provides a scope for resource names, ensuring uniqueness within that namespace.

* Namespaces ensure that names of resources (e.g., Deployments, Services) are unique within the namespace but not across namespaces.
    
    * Kubernetes starts with four initial namespaces:
        
        * default: Used for starting without creating a namespace.
            kube-node-lease: Holds Lease objects associated with each node.
        * kube-public: Readable by all clients (mostly reserved for cluster usage).
        * kube-system: For objects created by the Kubernetes system

## Pods

* Basic building blocks of kubernetes
* A wraper around a container, such that provides encapsulation of:
    * container
    * storage resources
    * unique network IP
    * options for running the container
* For MinIO, the pods are the containers that run the MinIO server and tenents: everytime you spin up a deployment for MinIO inside Kubernetes, there will be specific pods associated with that deployment

kind: Pod
metadata: 
    name: nginx         (pod name)
    namespace: minio    (namespace)
spec:  (what it pulls)
    containers:
    -   name: nginx
        image: nginx#

## Services
* An abstraction which defines a logical set of Pods and a policy by which to access them 
* For MinIO, the service is the way that we acess the MinIO server
* Two types of service:
    * ClusterIP: 
        * default service type
        * designed for internal comunication between pods whithin Kubernetes cluster
        * can be exposed externally using NodePort or Ingress

                apiVersion: v1
                kind: Service
                metadata: 
                    name:   nginx-clusterip # name of the service
                spec:
                    selector:
                        app: nginx-cip
                    ports:
                        - protocol: TPC
                        port: 80            # (listening to)
                        targetPort: 80      # (passing to)
                ---

                apiVersion: apps/v1




# documentation:
https://www.youtube.com/watch?v=Cra1Ipn0oYE&list=PLFOIsHSSYIK3mE_2m7i_7jdYu5CTebymN&index=5
