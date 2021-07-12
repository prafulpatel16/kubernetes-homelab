
Apply without creating 'twitter' namespce on master node and try creating a single pod

expected: it should show error

Actual: error occured " namespace twitter not found" as below

[root@master-node ~]# kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml
Error from server (NotFound): error when creating "https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml": namespaces "twitter" not found

Solution:
Create a namespace "twitter" and then try creating a pod. it will be succeded.


