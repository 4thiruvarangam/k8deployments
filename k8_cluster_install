### Configure the kernel modules and sysctl 
```text
   # sudo modprobe overlay
   # sudo modprobe br_netfilter
   # cat << EOF | sudo tee /etc/modules-load.d/k8s.conf
   overlay
   br_netfilter

   # cat << EOF | sudo tee /etc/sysctl.d/k8s.conf
   net.ipv4.ip_forward = 1 
   net.bridge.bridge-nf-call-iptables = 1
   net.bridge.bridge-nf-call-ip6tables = 1
   # sudo sysctl --system 
```
### Install and configure containerd for kubeadm
```text
   # sudo apt-get update
   # sudo apt-get install -y containerd
   # sudo mkdir -p /etc/containerd
   # containerd config default | sudo tee /etc/containerd/config.toml
   sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' \ 
   /etc/containerd/config.toml
   # sudo systemctl restart containerd
   # sudo systemctl enable containerd
   # crictl info 

```
### Configure crictl 
```text
   # sudo crictl config --set runtime-endpoint=unix:///run/containerd/containerd.sock
```

### Turn off the swap
```text
   # sudo swapoff -a 
   # sudo swapon --show
```


### Mark the kubernetes on hold
```text
   # sudo dpkg -l kubeadm kubectl kubelet 
   # sudo apt-mark hold kubeadm kubectl kubelet 
   # sudo apt-mark showhold 
```


### kubeadmin init 
```text
   # sudo kubeadm config print init-defaults > init.yaml
   # networking.podSubnet: 192.168.0.0/16
   # localAPIEndpoint.advertiseAddress: CP-IP
   # controlPlaneEndpoint: CP-IP:6443
   # apiServer.certSANs:
     - CP-IP
   # noderegistration.nodename: CP-NAME
   kubeadm join CP-IP:6443 --token <t> \
--discovery-token-ca-cert-hash sha256:<h>
   
```
## setting up the kubeadm
```text
    # mkdir -p /home/thiru/.kube
    # cp /etc/kubernetes/admin.conf /home/thiru/.kube/config
    # chown -R thiru:thiru /home/thiru/.kube/
```
## setting up the CNI network
```text
   # kubectl describe node control01 | grep -A 8 "Conditions"
   # kubectl get pods -n kube-system --show-labels
   # kubectl get pods -n kube-system -l k8s-app=kube-dns
   # bin_dir = "/opt/cni/bin"
   # sudo cp -p /etc/containerd/config.toml /etc/containerd/config.toml.bak-$(date +%Y%m%d-%H%M%S)
   # sudo sed -i 's|bin_dir = "/usr/lib/cni"|bin_dir = "/opt/cni/bin"|' /etc/containerd/config.toml
   # sudo systemctl restart containerd
   # curl -sI https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/tigera-operator.yaml| head -1
HTTP/2 200
   # kubectl create -f  https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/tigera-operator.yaml
   # kubectl create -f  https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/custom-resources.yaml

```
