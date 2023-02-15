# steps

```
git clone https://github.com/mmg10/k8s
cd k8s/setup
bash 01_container.sh
bash 02_kubetools.sh
```

```
sudo hostnamectl set-hostname control
sudo hostnamectl set-hostname worker1
sudo hostnamectl set-hostname worker2
```

# control node
```
sudo kubeadm init \
  --pod-network-cidr=10.100.0.0/16 \
  --node-name=control
```

# control node
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

# control plane - weave
```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

# OR control plane - calico
```
curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

# worker nodes; copy from terminal
```
sudo kubeadm join 172.31.25.12:6443 --token nzmidw.7kp3olmh1ymm7svr \
	--discovery-token-ca-cert-hash sha256:5b4c3f5d61bccec4af201d20a1ed742998116eb6334a716d25b4de5399e21dd7 
```

# or generate on control plane:
```
kubeadm token create --print-join-command
```
# verify on control plane
```
kubectl get nodes
```

# helm
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```
