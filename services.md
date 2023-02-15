# Services in K8s

k apply -f nginxdep.yaml


# run ubuntu to test access
apt install curl
curl 10.38.0.1:8080/test # ip address of pod on worker1; works
curl 10.40.0.2:8080/test # ip address of pod on worker2; works

# on host mahcine
curl 10.38.0.1:8080/test # ip address of pod on worker1; works
curl 10.40.0.2:8080/test # ip address of pod on worker2; works

# also works; here it is pod name
k port-forward express-test-56f65b9f85-5ncqf 2805:8080 & 
curl 127.0.0.1:2805/test


# create clusterip service 
k expose deploy express-test
k get svc # get clusterip

# on host mahcine
curl 10.108.164.206:8080/test # works
curl express-test:8080/test # fails since no dns resolution

# inside a pod
curl 10.108.164.206:8080/test # works
curl express-test:8080/test # works


# create NodePort
k expose deploy express-test --type=NodePort
-> all previous behavior remains.
curl http://54.213.61.45:32631/test # public IP of ANY node; works


# create Ingress
k expose deploy express-test # we need a service for ingress
k apply -f express-ingress.yaml
