# talent-arena-k8s-operator-workshop

Welcome to the K8S operator workshop! 

Learn how to build a custom Kubernetes operator in this hands-on workshop designed for
software infrastructure developers.
Discover the power of operators to automate complex tasks, manage application lifecycles,
and extend Kubernetes functionality with Golang based controllers.
Dive into best practices for managing resources and creating resilient, scalable solutions.

By the end of this workshop you will have built a working Kubernetes operator running on your laptop.

What you will learn:

* Writing custom controllers using the Kubebuilder
* Creating k8s custom resource definitions (CRDs)
* Managing CRD child resources
* Promote new ollama models based on a prompt response

## Local K8S setup

To complete this workshop you need to spawn a K8S cluster, which will be the one 
used to deploy the operator that you'll build. You are free to use whatever tool to 
spawn this local cluster, but we are suggesting [Kind](https://github.com/kubernetes-sigs/kind?tab=readme-ov-file)
since it is easy to install wherever you can run Docker. Make sure that Docker has at least 4GB of RAM.

Follow the steps below to install Kind in your laptop: 

1. [Install it using your preferred package manager](https://kind.sigs.k8s.io/docs/user/quick-start/#installing-with-a-package-manager)
2. Create a cluster using `kind create cluster --image kindest/node:v1.31.0`
3. Configure kubectl context to point to the new cluster `kubectl config use-context kind-kind`

   * In case that you don't have `kubectl` installed, follow the instructions [here](https://kubernetes.io/docs/tasks/tools/#kubectl)
 
4. Test your local cluster listing the existing pods `kubectl get pod -A`

   * You should see something like this:

```
NAMESPACE            NAME                                         READY   STATUS    RESTARTS   AGE
kube-system          coredns-668d6bf9bc-fnzhb                     1/1     Running   0          4m28s
kube-system          coredns-668d6bf9bc-rr748                     1/1     Running   0          4m28s
kube-system          etcd-kind-control-plane                      1/1     Running   0          4m32s
kube-system          kindnet-9mpbq                                1/1     Running   0          4m28s
kube-system          kube-apiserver-kind-control-plane            1/1     Running   0          4m32s
kube-system          kube-controller-manager-kind-control-plane   1/1     Running   0          4m32s
kube-system          kube-proxy-hkn88                             1/1     Running   0          4m28s
kube-system          kube-scheduler-kind-control-plane            1/1     Running   0          4m32s
local-path-storage   local-path-provisioner-58cc7856b6-862nh      1/1     Running   0          4m28s
```

---

Now that you have your local K8S cluster up & running you are ready to install
[kubebuilder](https://book.kubebuilder.io/introduction),
which is the tool we'll use to implement the K8S operator. When writing K8S operators we need
to interact with many different K8S APIs and Kubebuilder makes it super easy to work with those
APIs. Without it it would be more complex to write your own operator. 

## Kubebuilder setup

Feel free to follow the [Kubebuilder quick start guide on your own](https://book.kubebuilder.io/quick-start#quick-start)
or run the commands in the step below to create your first Kubebuilder project.

1. [Install kubebuilder](https://book.kubebuilder.io/quick-start#installation) 

```
# download kubebuilder and install locally.
curl -L -o kubebuilder "https://github.com/kubernetes-sigs/kubebuilder/releases/download/v4.5.0/kubebuilder_$(go env GOOS)_$(go env GOARCH)"
chmod +x kubebuilder && sudo mv kubebuilder /usr/local/bin/
```

2. Create the scaffolding for the kubebuilder project

```
mkdir -p ~/talent-arena-operator
cd ~/talent-arena-operator
kubebuilder init --domain talent.arena --repo talent.arena/operator
```

You can now open the current directory in your preferred code editor. You should see the following 
file tree.

![kubebuilder init](/img/kubebuilder_scaffolding.png)

You are ready to start [Part One](./docs/part_one.md)!

### Recommended versions

These are the versions we have used for the workshop and we've validated that it was working correctly.
Latest versions should also work fine, but in case that you find any issue you could use these ones:

* Go version go1.23
* Kubebuilder version: 4.5.0
* Kubernetes version: 1.31.0
* Kubectl version: v1.30.0
   