# Empty up Nodes
- kubectl drain <NODE_NAME> --ignore-daemonsets
- ! Won't work with pod scheduled that are not part of ReplicaSets.
# Restoring Node
- kubectl uncordon <NODE_NAME>
- ! Only when the pod will re-created, they will be re-scheduled
# Stop Scheduling Pod on specific Node
- k cordon <NODE_NAME>

# Cluster upgrade process:

## Compatibility:
1. Kube-API - 1.10
2. Controller-Manager & Kube-Scheduler x-1
3. Kubelet & Kube-Proxy x-2
4. KubeCTL (X+1,X-1)
## Order:
1. One minor version at the time
2. Kubeadm for upgrading
3. First - The master is upgrading

4. Strategies:
- Rolling upgrade strategy (One By One).
- Adding new nodes to the cluster

5. Kubeadm upgrade plan
- ! Kubeadm dosn't upgrade kubelet
6. Upgrade kubeadm:
- apt-get upgrade -y kubeadm=<version>
7. Upgrade kubelet
- apt-get upgrade -y kubelet=<version>
- systemctl restart kubelet

8. Steps:
- Drain <NODE>

### Installing Kubeadm package in Deb
Use any text editor you prefer to open the file that defines the Kubernetes apt repository.

vim /etc/apt/sources.list.d/kubernetes.list
Update the version in the URL to the next available minor release, i.e v1.34.

deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.34/deb/ /
After making changes, save the file and exit from your text editor. Proceed with the next instruction.

apt update

apt-cache madison kubeadm

- apt-get upgrade -y kubeadm=<version> OR apt-get install kubeadm=<version>


### Upgrade Kubeadm components:
- kubeadm upgrade plan <version>
- kubeadm upgrade apply <version>

### Upgrade Kubelet:
apt-get install kubelet=1.34.0-1.1
systemctl daemon-reload
systemctl restart kubelet

# Reference for upgrading:
- Control Plane :``` https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#upgrading-control-plane-nodes```
- Kubelet & Kubectl : ```https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#upgrade-kubelet-and-kubectl```
- Worker Nodes: ```https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#upgrade-worker-nodes```
