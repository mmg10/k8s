# Dashboard


# metrics-server
```
curl https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
add "- --kubelet-insecure-tls" in spec -> containers -> args
kubectl apply -f components.yaml
```

# skooner
```
wget https://raw.githubusercontent.com/skooner-k8s/skooner/master/kubernetes-skooner.yaml  

# change type to NodePort  
k apply -f kubernetes-skooner.yaml  

kubectl create serviceaccount skooner-sa
kubectl create clusterrolebinding skooner-sa --clusterrole=cluster-admin --serviceaccount=default:skooner-sa
kubectl get secrets # empty now
k create token skooner-sa
```
