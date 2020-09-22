# kubernetes-Install-Threenode-Cluster
How to Install Three node K8s cluster for on premise or local Virtual Machine for Ubuntu 
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
  
