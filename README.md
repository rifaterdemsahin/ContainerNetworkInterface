Here’s a markdown page for setting up **Minikube** on **GitHub Codespaces** to test Container Network Interfaces (CNI) with 10 use cases, complete with code snippets and emojis:

---

# 🐳 Minikube on GitHub Codespaces to Test CNIs 🛠️

Minikube is a great way to test Container Network Interfaces (CNIs) in a lightweight Kubernetes cluster. Here's how to set it up on GitHub Codespaces.

## 📝 Prerequisites

- A GitHub account with access to Codespaces
- Codespaces enabled on your repository
- Docker installed on Codespaces

## 🚀 Steps to Set Up Minikube

### 1️⃣ Install Minikube
First, install Minikube on your Codespace:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### 2️⃣ Start Minikube
You need Docker to run Minikube on Codespaces. Use Docker as the driver to start the Minikube cluster:

```bash
minikube start --driver=docker
```

Check that your cluster is up and running:

```bash
minikube status
```

### 3️⃣ Enable Kubernetes Dashboard
For a graphical interface, enable the Kubernetes dashboard:

```bash
minikube dashboard --url
```

You’ll get a link to access the dashboard! 🎉

---

## 🔥 10 Use Cases to Test CNIs

### 1️⃣ Default CNI Plugin (bridge)
Test Minikube with the default CNI plugin:

```bash
kubectl apply -f https://k8s.io/examples/pods/simple-pod.yaml
```

The default plugin sets up basic networking to allow pod-to-pod communication. 🧑‍💻

### 2️⃣ Install and Test Calico CNI
Calico provides network security policies and enhances pod-to-pod communication.

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

Check the status of the Calico installation:

```bash
kubectl get pods -n kube-system | grep calico
```

### 3️⃣ Test Flannel CNI
Flannel provides a simple overlay network for Kubernetes.

```bash
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```

Ensure all nodes are using Flannel:

```bash
kubectl get nodes -o wide
```

### 4️⃣ Network Policy Testing with Calico 🛡️
Define and enforce a network policy that restricts communication between namespaces:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: deny-other-ns
  namespace: default
spec:
  podSelector: {}
  ingress:
  - from:
    - podSelector: {}
```

Apply the policy:

```bash
kubectl apply -f network-policy.yaml
```

### 5️⃣ Test Multus CNI for Multi-Interface Pods 🌐
Multus allows a pod to have multiple interfaces.

Install Multus:

```bash
kubectl apply -f https://raw.githubusercontent.com/k8snetworkplumbingwg/multus-cni/master/images/multus-daemonset.yml
```

Create a multi-network pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-net-pod
  annotations:
    k8s.v1.cni.cncf.io/networks: my-secondary-network
spec:
  containers:
  - name: my-pod
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
```

### 6️⃣ Test Weave CNI 🕸️
Weave simplifies Kubernetes networking and offers encryption.

```bash
kubectl apply -f https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')
```

Check that Weave is working:

```bash
kubectl get pods -n kube-system -l name=weave-net
```

### 7️⃣ Test Cilium CNI 🐍
Cilium uses eBPF to provide networking, security, and observability.

```bash
kubectl apply -f https://github.com/cilium/cilium/blob/master/install/kubernetes/quick-install.yaml
```

Validate Cilium installation:

```bash
kubectl get pods -n kube-system -l k8s-app=cilium
```

### 8️⃣ Test Kube-Router CNI 🚦
Kube-router provides network routing, firewalling, and NAT.

```bash
kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/kubeadm-kuberouter-all-features.yaml
```

Verify Kube-Router is running:

```bash
kubectl get pods -n kube-system | grep kube-router
```

### 9️⃣ Test Macvlan CNI for Bare-Metal IP Assignment 🌍
Macvlan allows pods to be assigned IP addresses directly from the host’s network.

Create a Macvlan network:

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: macvlan-pod
spec:
  containers:
  - name: macvlan
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
EOF
```

### 🔟 Test SR-IOV CNI for High-Performance Networking 🚀
SR-IOV is used for high-performance networking in Kubernetes.

Install the SR-IOV plugin:

```bash
kubectl apply -f https://github.com/intel/sriov-cni/blob/master/images/sriov-daemonset.yaml
```

Deploy an SR-IOV-enabled pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sriov-pod
spec:
  containers:
  - name: sriov-container
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
```

---

## 📚 Conclusion

By following these steps, you'll be able to test and explore various CNIs using Minikube on GitHub Codespaces. 🚀 Happy learning!

---

