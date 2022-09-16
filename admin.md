# Create a kubernetes cluster
```python
sudo kubeadm init --apiserver-advertise-address=xxx.xxx.xx.xx --pod-network-cidr=xxx.xxx.0.0/16
```

# Reset the cluster
```python
sudo kubeadm reset
```

# Print the join command
- kubeadm token create --print-join-command
