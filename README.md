# kubernetes-Install-Threenode-Cluster
How to Install Three node K8s cluster for on premise or local Virtual Machine for Ubuntu 

![Image of kubernetes](https://github.com/sada498/kubernetes-Install-Threenode-Cluster/blob/master/kubernetes.PNG)

## Run below commands on all nodes to configure the k8s 
### Get the Docker gpg key
  ```
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 ```
### Add the Docker repository
  ```
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
  ```
### Get the Kubernetes gpg key
  ```
  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
  ```
### Add the Kubernetes repository
  ```
  cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
  deb https://apt.kubernetes.io/ kubernetes-xenial main
  EOF
  ```
### Update your packages
  ```
  sudo apt-get update
  ```
### Install Docker, kubelet, kubeadm, and kubectl
  ```
  sudo apt-get install -y docker-ce=18.06.0~ce~3-0~ubuntu kubelet=1.18.5-00 kubeadm=1.18.5-00 kubectl=1.18.5-00 
  ```
### Hold them at the current version
  ```
  sudo apt-mark hold docker-ce kubelet kubeadm kubectl
  ```
### Add the iptables rule to sysctl.conf
  ```
  echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
  ```
### Enable iptables immediately
  ```
  sudo sysctl -p
  ```
  # Run below codes only on Master node
### Initialize the cluster
  ```
  sudo kubeadm init --pod-network-cidr=10.244.0.0/16
  ```
  ![Image of kube join](https://github.com/sada498/kubernetes-Install-Threenode-Cluster/blob/master/kube%20join.png)
  copy the kube join command for join the worker node into cluster. use this command only on worker nodes 
### Set up local kubeconfig
  ```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  ```
### Apply Calco CNI
  ```
  kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
  ```
## join the worker nodes into master node only run this command in worker node 
![Image of kube join to worker node ](https://github.com/sada498/kubernetes-Install-Threenode-Cluster/blob/master/worker%20node%20join.PNG)

## Finally, check the master for worker nodes.
  ```
   alias kc=kubectl
   kc get nodes
  ```
  ![Image of master](https://github.com/sada498/kubernetes-Install-Threenode-Cluster/blob/master/kc%20get%20nodes.png)


