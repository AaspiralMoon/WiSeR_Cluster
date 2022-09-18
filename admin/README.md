
# 

<details>
  <summary>Create a kubernetes cluster</summary>
  ```python
  $ sudo kubeadm init --apiserver-advertise-address=xxx.xxx.xx.xx --pod-network-cidr=xxx.xxx.0.0/16
  ```
</details>


# Reset the cluster
```python
$ sudo kubeadm reset
```

# Print the join command
```python
$ kubeadm token create --print-join-command
```
