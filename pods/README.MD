
1_ singlenode.yml

Action: 1
Apply without creating 'twitter' namespce on master node and try creating a single pod

expected: it should show error

Actual: error occured " namespace twitter not found" as below

[root@master-node ~]# kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml
Error from server (NotFound): error when creating "https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml": namespaces "twitter" not found

Solution:
Create a namespace "twitter" and then try creating a pod. it will be succeded.

Actions: 2

1.Creeate a "twitter" namespace first

kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/namespace/namespace_2.yml

[root@master-node ~]# kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/namespace/namespace_2.yml
namespace/twitter created
namespace/facebook created

[root@master-node ~]# kubectl get ns
NAME              STATUS   AGE
default           Active   29h
facebook          Active   84s
kube-node-lease   Active   29h
kube-public       Active   29h
kube-system       Active   29h
twitter           Active   84s

2. now create a single pod

kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml

[root@master-node ~]# kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml
pod/webserver created

3. check all details from twitter namespace
 

[root@master-node ~]# kubectl get all -n twitter
NAME            READY   STATUS    RESTARTS   AGE
pod/webserver   1/1     Running   0          2m15s

4. Let's check the twitter namespace pod in more details with labels
-l = stands for lablel

[root@master-node ~]# kubectl get all -n twitter
NAME            READY   STATUS    RESTARTS   AGE
pod/webserver   1/1     Running   0          2m15s
[root@master-node ~]# kubectl get all -n twitter -l app=nginx
NAME            READY   STATUS    RESTARTS   AGE
pod/webserver   1/1     Running   0          9m3s
[root@master-node ~]# kubectl get all -n twitter -l tier=front
NAME            READY   STATUS    RESTARTS   AGE
pod/webserver   1/1     Running   0          9m27s
[root@master-node ~]# kubectl get all -n twitter -l version=v1
NAME            READY   STATUS    RESTARTS   AGE
pod/webserver   1/1     Running   0          9m41s
[root@master-node ~]# kubectl get all -n twitter -l env=production
NAME            READY   STATUS    RESTARTS   AGE
pod/webserver   1/1     Running   0          9m50s

5. check all details of pos using -o wide option
[root@master-node ~]# kubectl get all -n twitter -o wide
NAME            READY   STATUS    RESTARTS   AGE   IP               NODE       NOMINATED NODE   READINESS GATES
pod/webserver   1/1     Running   0          20m   192.168.226.65   worker-1   <none>           <none>






