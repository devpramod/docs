# Getting Started with Kubernetes for ChatQnA

## Introduction
Kubernetes is an orchestration platform for managing containerized applications, ideal for deploying microservices based architectures like ChatQnA. It offers robust mechanisms for automating deployment, scaling, and operations of application containers across clusters of hosts. Kubernetes supports different deployment modes for ChatQnA, which cater to various operational preferences:

- **Using GMC (GenAI Microservices Connector)**: GMC can be used to compose and adjust GenAI pipelines dynamically on kubernetes for enhanced service connectivity and management.
- **Using Manifests**: This involves deploying directly using Kubernetes manifest files without the GenAI Microservices Connector (GMC).
- **Using Helm Charts**: Facilitates deployment through Helm, which manages Kubernetes applications through packages of pre-configured Kubernetes resources.

This guide will provide detailed instructions on using these resources. If you're already familiar with Kubernetes, feel free to skip ahead to [Helm Deployment](./k8s_helm.md)

### Kubernetes Cluster and Development Environment

**Setting Up the Kubernetes Cluster:** Before beginning deployment for the ChatQnA application, ensure that a Kubernetes cluster is ready. For guidance on setting up your Kubernetes cluster, please refer to the comprehensive setup instructions available on the [Opea Project deployment guide](https://opea-project.github.io/latest/deploy/index.html).

**Development Pre-requisites:** To prepare for the deployment, familiarize yourself with the necessary development tools and configurations by visiting the [GenAI Infrastructure development page](https://opea-project.github.io/latest/GenAIInfra/DEVELOPMENT.html). This page covers all the essential tools and settings needed for effective development within the Kubernetes environment.


**Understanding Kubernetes Deployment Tools and Resources:**

- **kubectl**: This command-line tool allows you to deploy applications, inspect and manage cluster resources, and view logs. For instance, `kubectl apply -f chatqna.yaml` would be used to deploy resources defined in a manifest file.
    
- **Pods**: Pods are the smallest deployable units created and managed by Kubernetes. A pod typically encapsulates one or more containers where your application runs.

**Verifying Kubernetes Cluster Access with kubectl**
```bash
kubectl get nodes
```

This command lists all the nodes in the cluster, verifying that `kubectl` is correctly configured and has the necessary permissions to interact with the cluster.

Some commonly used kubectl commands and their functions that will help deploy ChatQnA successfully:
|Command                          |Function                     |
|-------------------------------  |-----------------------------|
|`kubectl describe pod <pod-name>` | Provides detailed information about a specific pod, including its            current state, recent events, and configuration details.                            |
|`kubectl delete deployments --all` |             Deletes all deployments in the current namespace, which effectively removes all the managed pods and associated resources.                |
|`kubectl get pods -o wide`         |              Retrieves a detailed list of all pods in the current namespace, including additional information like IP addresses and the nodes they are running on.               |
|`kubectl logs <pod-name>`         |         Fetches the logs generated by a container in a specific pod, useful for debugging and monitoring application behavior.                    |
|`kubectl get svc`                  |            Lists all services in the current namespace, providing a quick overview of the network services and their status.

#### Create and Set Namespace
A Kubernetes namespace is a logical division within a cluster that is used to isolate different environments, teams, or projects, allowing for finer control over resources and access management. To create a namespace called `chatqa`, use:
```bash
kubectl create ns chatqa
```
When deploying resources (like pods, services, etc.) into your specific namespace, use the `--namespace` flag with `kubectl` commands, or specify the namespace in your resource configuration files.

To deploy a pod in the `chatqa` namespace:
```bash
kubectl apply -f your-pod-config.yaml --namespace=chatqa
```
If you want to avoid specifying the namespace with every command, you can set the default namespace for your current context:
```bash
kubectl config set-context --current --namespace=chatqa
```

### Using Helm Charts to Deploy

**What is Helm?** Helm is a package manager for Kubernetes, similar to how apt is for Ubuntu. It simplifies deploying and managing Kubernetes applications through Helm charts, which are packages of pre-configured Kubernetes resources.

**Key Components of a Helm Chart:**

- **Chart.yaml**: This file contains metadata about the chart such as name, version, and description.
- **values.yaml**: Stores configuration values that can be customized depending on the deployment environment. These values override defaults set in the chart templates.
- **deployment.yaml**: Part of the templates directory, this file describes how the Kubernetes resources should be deployed, such as Pods and Services.

**Update Dependencies:**

- A script called **./update_dependency.sh** is provided which is used to update chart dependencies, ensuring all nested charts are at their latest versions.
- The command `helm dependency update chatqna`  updates the dependencies for the `chatqna` chart based on the versions specified in `Chart.yaml`.

**Helm Install Command:**

- `helm install [RELEASE_NAME] [CHART_NAME]`: This command deploys a Helm chart into your Kubernetes cluster, creating a new release. It is used to set up all the Kubernetes resources specified in the chart and track the version of the deployment.

For more detailed instructions and explanations, you can refer to the [official Helm documentation](https://helm.sh/docs/).
