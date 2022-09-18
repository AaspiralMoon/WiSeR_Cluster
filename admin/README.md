
# Commands for admin

<details>
  <summary>Create a kubernetes cluster</summary>
  <pre><code> 
  $ sudo kubeadm init --apiserver-advertise-address=xxx.xxx.xx.xx --pod-network-cidr=xxx.xxx.0.0/16
  </code></pre> 
</details>


# Reset the cluster
```python
$ sudo kubeadm reset
```

# Print the join command
```python
$ kubeadm token create --print-join-command
```
