# Commands for admin

<details>
  <summary>Create a kubernetes cluster</summary>
  
  ```python
  sudo kubeadm init --apiserver-advertise-address=xxx.xxx.xx.xx --pod-network-cidr=xxx.xxx.0.0/16
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
  sudo iptables -F && sudo iptables -t nat -F && sudo iptables -t mangle -F && sudo iptables -X && sudo ipvsadm -C
  ```
</details>


<details>
  <summary>Print the join command</summary>
  
  ```python
  kubeadm token create --print-join-command
  ```
</details>

