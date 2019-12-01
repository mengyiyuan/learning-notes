# Set up a Kubernetes cluster with 1 master and 2 nodes (Flannel network)

[Reference](https://linuxacademy.com/cp/courses/lesson/course/3720/lesson/1/module/305)

## On all 3 servers

1. Set up the Docker and Kubernetes repositories:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

2. Install Docker and Kubernetes packages:

Note that if you want to use a newer version of Kubernetes, change the version installed for kubelet, kubeadm, and kubectl. **Make sure all three use the same version.**

```
sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu kubelet=1.13.5-00 kubeadm=1.13.5-00 kubectl=1.13.5-00

sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```

3. Enable iptables bridge call:
```
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p
```

## On the Kube master server only
1. Initialize the cluster:
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

Note: `--pod-network-cidr=10.244.0.0/16` is required by Flannel, [reference](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network)

2. Set up local kubeconfig:
```
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

```

3. Install Flannel networking:
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```

## On each Kube node server
1. Join the node to the cluster:
```
sudo kubeadm join $controller_private_ip:6443 --token $token --discovery-token-ca-cert-hash $hash
```

**Note**: substitute the values of `$controller_private_ip` and `$hash` with actual values by following the instructions shown after running `kubeadm init` on Kube master.

## On the Kube master server
1. Verify that all nodes are joined and ready:
```
kubectl get nodes
```

**Note**: do not attempt to run this command on kube nodes, it will give error "The connection to the server localhost:8080 was refused - did you specify the right host or port?", because kube nodes don't have access to `kube-apiserver`.