## Manual Kubernetes Setup Guide

### Prerequisites
- Set up two Linux machines:
  - One machine will be the Kubernetes master node.
  - The other will be the Kubernetes worker node.

### Step 1: Install Docker
- Install Docker on both machines following the official Docker documentation for your operating system.

### Step 2: Install Kubernetes Components
On both the master and worker nodes, install the following components:

```bash
# Install kubelet, kubeadm, kubectl on both nodes
sudo apt-get update && sudo apt-get install -y kubelet kubeadm kubectl
```

### Step 3: Initialize the Master Node
On the master node, run the following command:

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

This will set up the Kubernetes control plane components on the master node. After the initialization is complete, `kubeadm` will provide a command to join worker nodes to the cluster.

### Step 4: Configure kubectl on the Master Node
Once the master node initialization is successful, you need to set up `kubectl` configuration. Run the following commands:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Step 5: Join the Worker Node to the Cluster
On the worker node, use the command provided by `kubeadm init` to join the node to the cluster:

```bash
sudo kubeadm join <master-node-ip>:<master-node-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

### Step 6: Set up a Networking Solution
Kubernetes needs a networking solution to enable communication between pods across nodes. Install your preferred networking solution on the master node:

```bash
# Example: Install Calico networking
kubectl apply -f https://docs.projectcalico.org/v3.18/manifests/calico.yaml
```

### Step 7: Verify the Cluster Status
On the master node, check if all components are running and ready:

```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

Ensure that all nodes and control plane components are in the "Ready" state.

That's it! You should now have a simple Kubernetes cluster set up with one master and one worker node. However, please keep in mind that this is a basic setup, and for production environments, you should consider using more advanced configurations, high availability, security measures, etc. Manual setup can be error-prone, and it's generally recommended to use automated tools like `kubeadm` or infrastructure-as-code solutions for more complex setups.
