# kubernetes 🚢

## Install Docker on Ubuntu
The first step is to install docker on every node using `apt install docker.io` and ensure that it is enabled to start after reboot using `systemctl enable docker`
## Install Kubernetes
- Execute the below command on all nodes (**Master & Slave**) to install Kubernetes.
- Adding the Kubernetes signing key : `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add`
- Add the Kubernetes repository : `apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"`
- Install Kubernetes : `apt install -y kubeadm kubelet kubectl`
## Initialize Kubernetes Master Node
- First lets label the Master Node : `hostnamectl set-hostname kubernetes-master`
- Initialise the Master server using `kubeadm init --pod-network-cidr=10.244.0.0/16`
- Take a note of the entire kubeadm join command from the bottom of the above Kubernetes master node initialization output as you will use this command later when joining the Kubernetes cluster with your slave nodes.
- Run the below commands as per the output you see on the screen
```
kubernetes-master:~$ mkdir -p $HOME/.kube
kubernetes-master:~$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
kubernetes-master:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
- Deploy the pod network using 
```
kubernetes-master:~$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubernetes-master:~$ kubectl get pods --all-namespaces
```
## Initialize Kubernetes Worker Node
- Label the Worker Node : `hostnamectl set-hostname kubernetes-worker`
- Now run the `kubeadm join <IP>:6443 --token XXXXXYYYYYYYZZZZZZZ`
## Check the installation
To check whether we have one master node and one worker node added to the cluster : `kubectl get nodes`
