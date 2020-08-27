### **Minikube cheatsheet**
* `minikube start` to start minikube cluster
* `minikube start --kubernetes-version v1.18.0` to start minikube with a specific kubernetes version
* `minikube dashboard` to load the dashboard
* `minikube status` to get the cluster status
* `minikube addons list` to list all the addons
* `minikube addons (enable|disable) <addone-name>` to enable/disable the addon
* `minikube stop` to stop the cluster
* `minikube delete` to delete the cluster
* `minikube start --driver=<driver_name>` with a specific VM driver

  > Caution: If you use the none driver, some Kubernetes components run as privileged containers that have side effects outside of the Minikube environment. Those side effects mean that the none driver is not recommended for personal workstations.

### **Kubectl cheatsheet**
* `kubectl version` get the kubectl cli version
* `kubectl get (pods|services|svc|deployments|nodes|events|replicasets)` to get all the pods|services|deployments|nodes running
* `kubectl get pod,svc -n kube-system` to view the pod and services created****
* `kubectl config view` to view kubectl configuration
* `kubectl create deployment <deployment-name> --image=<docker-image-path>:<tag>` to create a new deployment on a new node
* `kubectl expose deployment <deployment-name> --type=LoadBalancer --port=8000` to expose the pod to public internet. `--type=LoadBalancer` flag indicates that you want to expose your service outside of the cluster.
* `kubectl proxy` to forward communications into the cluster-wise, private network. The proxy can be terminated by ctrl+C and wont show any output while it is running.
* `kubectl scale deployments/<deployment-name> --replicas=<replica-count>` to scale the replica
* `kubectl set image deployments/<deployment-name> <deployment-name>=<NewOrOld-deployment-image>:<tag-name>` to apply the rolling update
   * `kubectl rollout status deployments/<deployment-name>` to check the rollout status
   * `kubectl rollout undo deployments/<deployment-name>` to rollout a failed deployment (invalid image deployment)
* `kubectl delete service <service-name>` to delete the service
* `kubectl delete deployment <service-name>` to delete the deployment
* `kubectl apply -f deployment.yaml` to create a deployment using a deployment.yaml file
***Troubleshoot***
* `kubectl describe (pods|services|replicasets)[\name]` - show detailed information about a resource
* `kubectl logs <pod_name>` - print the logs from a container in pod
* `kubectl exec <pod_name> <command>` - execute a command on a container in a pod
   * `kubectl exec <pod_name> env` list env variables
   * `kubectl exec -ti <pod_name> bash` start a bash session in the pod's container. here you can execute any shell commands i.e. if your app is node app you can print the file contents, or run the curl command to see the output of your web app, etc.


**Sample Deployment YAML**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/name: load-balancer-example
  name: hello-world
spec:
  replicas: 5
  selector:
    matchLabels:
      app.kubernetes.io/name: load-balancer-example
  template:
    metadata:
      labels:
        app.kubernetes.io/name: load-balancer-example
    spec:
      containers:
      - image: gcr.io/google-samples/node-hello:1.0
        name: hello-world
        ports:
        - containerPort: 8080
```

**Terminology**
* **Master**: The Master is responsible for managing the cluster. A Kubernetes cluster can be deployed on either physical or virtual machines.
* **Node**: A node is a VM or a physical computer that serves as a worker machine in a Kubernetes cluster. The nodes communicate with the master using the Kubernetes API, which the master exposes
  * **Node processes**: runs inside a node
* **Pod**: A Pod is a basic execution unit of a Kubernetes application. Each Pod represent a part of a workload that is running on your cluster. They are visible and can be accessed from within the cluster by other pods and services but not outside the network but we can expose it to the internet. Each pod has a unique IP address even for Pods on the same Node. [Learn more](https://kubernetes.io/docs/concepts/workloads/pods/)
* **Service**: A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. Services enable a loose coupling between dependent Pods. A Service is defined using YAML (preferred) or JSON, like all Kubernetes objects. The set of Pods targeted by a Service is usually determined by a LabelSelector.
   **`Types`**
   * *ClusterIP (default)*: Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
   * *NodePort*: Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
   * *LoadBalancer*: Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
   * *ExternalName*: Exposes the Service using an arbitrary name (specified by `externalName` in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of `kube-dns`.
* **Label**: Labels are key/value pairs attached to objects and can be used in any number of ways:
  * Designate objects for development, test and production
  * Embed version tags
  * Classify an object using tags

---

## [Getting started](https://kubernetes.io/docs/setup/) on your local dev box

**Pre-requisites**
1. Docker
2. Hyper-V (hyper-v enabled on your device)

<br>---

**Kubernetes Cluster**
![Cluster Diagram](https://d33wubrfki0l68.cloudfront.net/99d9808dcbf2880a996ed50d308a186b5900cec9/40b94/docs/tutorials/kubernetes-basics/public/images/module_01_cluster.svg)

---

**Node Overview**
![Node Overview](https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg)

---

**Pods Overview**
![Pods Overview](https://d33wubrfki0l68.cloudfront.net/fe03f68d8ede9815184852ca2a4fd30325e5d15a/98064/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)

---

**Services**
![Services and Labels](https://d33wubrfki0l68.cloudfront.net/cc38b0f3c0fd94e66495e3a4198f2096cdecd3d5/ace10/docs/tutorials/kubernetes-basics/public/images/module_04_services.svg)
**Labels**
![Services and Labels](https://d33wubrfki0l68.cloudfront.net/b964c59cdc1979dd4e1904c25f43745564ef6bee/f3351/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)

---
**Scaling & Overview 1**
![Scaling overview 1](https://d33wubrfki0l68.cloudfront.net/043eb67914e9474e30a303553d5a4c6c7301f378/0d8f6/docs/tutorials/kubernetes-basics/public/images/module_05_scaling1.svg)
**Scaling & Overview 2**
![Scaling overview 2](https://d33wubrfki0l68.cloudfront.net/30f75140a581110443397192d70a4cdb37df7bfc/b5f56/docs/tutorials/kubernetes-basics/public/images/module_05_scaling2.svg)

---

**Installation**
1. Install Docker
2. [Install Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

   Read the [Kubectl reference](https://kubernetes.io/docs/reference/kubectl/) for more details
   Verify installation by running `kubectl version --client`.
   ###### Note: [Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/#kubernetes) adds its own version of `kubectl` to PATH. If you have installed Docker Desktop before, you may need to place your PATH entry before the one added by the Docker Desktop installer or remove the Docker Desktop's `kubectl`.
   ### Verify kubectl configuration
   Verify if the [kubeconfig file](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) is present. Else it will not work.
   Check that kubectl is properly configured by getting the cluster state
   ```
   kubectl cluster-info
   ```
   If kubectl cluster-info returns the url response but you can't access your cluster, to check whether it is configured properly, use:
   ```
   kubectl cluster-info dump
   ```
3. [Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/). You can also install [Kind](https://kubernetes.io/docs/setup/learning-environment/kind/) to work on clusters with Kubernetes
   ###### Pre-requisite check if virtualization is supported
   * Install minikube either by downloading binary/executable or using choco if you are on windows
   * Confirm installation by checkking `minikube version`
   * Start minikube using `minikube start`. If you have a specific driver to specify, run `minikube start --driver=<driver-name>
   * Check minikube status after starting, which will show below output if everything is fine
    ```
    host: Running
    kubelet: Running
    apiserver: Running
    kubeconfig: Configured
    ```
    ### Clean up local state
    If you have previously installed Minikube, and try to start it which returns an error saying `minikube does not exist`, then you need to delete minikube's local state: `minikube delete`

**Minikube with Kubernetes**
1. Start minikube cluster `minikube start`.
  [Click here](https://kubernetes.io/docs/setup/learning-environment/minikube/#starting-a-cluster) to start a cluster on a specific Kubernetes version, VM or container runtime.
2. [Interact with your cluster](https://kubernetes.io/docs/setup/learning-environment/minikube/#interacting-with-your-cluster) with kubectl: `kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10`
3. Access hello-minikube deployment, expose it as a service: `kubectl expose deployment hello-minikube --type=NodePort --port=8080`
4. Check if the pod is running: `kubectl get pod`
5. Get the URL of the exposed service: `minikube service hello-minikube --url`
6. Copy over the URL in a browser to get the result of the service
7. Delete the `hello-minikube` service: `kubectl delete services hello-minikube`
8. Delete the `hello-minikube` deployment: `kubectl delete deployment hello-minikube`
9. Stop the local minikube cluster: `minikube stop`
10. Delete the local minikube cluster: `minikube delete`

#### Drivers available as of Aug 2020.
  * docker ([docker installation](https://minikube.sigs.k8s.io/docs/drivers/docker/))
  * virtualbox ([driver insallation](https://minikube.sigs.k8s.io/docs/drivers/virtualbox/))
  * podman ([driver installation](https://minikube.sigs.k8s.io/docs/drivers/podman/) ***EXPERIMENTAL)
  * vmwarefusion
  * kvm2 ([driver installation](https://minikube.sigs.k8s.io/docs/reference/drivers/kvm2/))
  * hyperkit ([driver installation](https://minikube.sigs.k8s.io/docs/reference/drivers/hyperkit/))
  * hyperv ([driver installation](https://minikube.sigs.k8s.io/docs/reference/drivers/hyperv/)) Note that the IP below is dynamic and can change. It can be retrieved with `minikube ip`
  * vmware ([driver installation](https://minikube.sigs.k8s.io/docs/reference/drivers/vmware/)) (VMware unified driver)
  * parallels ([driver installation](https://minikube.sigs.k8s.io/docs/reference/drivers/parallels/))
  * none (Runs the Kubernetes components on the host and not in a virtual machine. You need to be running Linux and to have Docker installed.)
