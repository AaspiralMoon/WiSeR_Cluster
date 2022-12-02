# Commands for admin

<details>
  <summary>Create a kubernetes cluster</summary>
  
  ```python
  sudo kubeadm init --apiserver-advertise-address=xxx.xxx.xx.xx --pod-network-cidr=192.168.0.0/16
  ```
</details>


<details>
  <summary>Reset a kubernetes cluster</summary>
  
  ```python
  # run on master nodes and worker nodes
  sudo kubeadm reset -f
  sudo rm -rf /etc/cni/net.d && sudo rm -rf $HOME/.kube/config
  sudo iptables -F && sudo iptables -t nat -F && sudo iptables -t mangle -F && sudo iptables -X
  ```
</details>

<details>
  <summary>Remove a node</summary>
  
  ```python
  # run on master nodes
  kubectl drain <node name> --delete-emptydir-data --force --ignore-daemonsets
  kubectl delete node <node name>
  
  # run on worker nodes
  sudo kubeadm reset -f
  sudo rm -rf /etc/cni/net.d && sudo rm -rf $HOME/.kube/config
  sudo iptables -F && sudo iptables -t nat -F && sudo iptables -t mangle -F && sudo iptables -X
  ```
</details>


<details>
  <summary>Print the join command</summary>
  
  ```python
  kubeadm token create --print-join-command
  ```
</details>

<details>
  <summary>Configure a NFS server in master node</summary>
  - In master node
  
  ```python
  sudo apt update && sudo apt install nfs-kernel-server # install nfs server dependencies
  sudo mkdir -p /mnt/nfs_host # create a directory for file sharing
  sudo chown -R nobody:nogroup /mnt/nfs_host/ # remove restrictions
  sudo chmod 777 /mnt/nfs_host/ # give read/write permissions
  sudo nano /etc/exports
  /mnt/nfs_host  130.113.70.171(rw,sync,no_subtree_check) # include the ip addresses of all clients
  sudo exportfs -a # export configuration
  sudo systemctl restart nfs-kernel-server # finish configuring restart the nfs services
  ```
  
  - In worker node
  ```python
  sudo apt update && sudo apt install nfs-common # install nfs client dependencies
  
  # Optional
  sudo mkdir -p /mnt/nfs_client # create local directory that will be connected with the server
  sudo mount 130.113.70.172:/mnt/nfs_host  /mnt/nfs_client # connect these two directories
  ```
</details>
