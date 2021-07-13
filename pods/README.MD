
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


6. List all pods in all namespaces
 
 [root@master-node ~]# kubectl get pods --all-namespaces
NAMESPACE     NAME                                       READY   STATUS             RESTARTS   AGE
kube-system   calico-kube-controllers-65f8bc95db-47dxv   1/1     Running            3          32h
kube-system   calico-node-494qf                          1/1     Running            4          32h
kube-system   calico-node-vgbbx                          1/1     Running            2          32h
kube-system   coredns-66bff467f8-bz64t                   1/1     Running            3          32h
kube-system   coredns-66bff467f8-sl5hp                   1/1     Running            3          32h
kube-system   etcd-master-node                           1/1     Running            3          32h
kube-system   kube-apiserver-master-node                 1/1     Running            3          32h
kube-system   kube-controller-manager-master-node        1/1     Running            49         32h
kube-system   kube-proxy-8gjv4                           1/1     Running            3          32h
kube-system   kube-proxy-b6gjm                           1/1     Running            2          32h
kube-system   kube-scheduler-master-node                 1/1     Running            43         32h
twitter       mysql                                      0/1     CrashLoopBackOff   8          18m
twitter       webserver                                  1/1     Running            0          3h2m


7. Describe pod in details in twitter namespace
kubectl describe pod/webserver -n twitter

kubectl describe pod/myswl -n twitter

8. Delete pods webserver and database from twitter namespaces from github yaml automate
 
 Delete webserver pod
 kubectl delete -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/1_singlepod.yml
 
 Delete database pod
 kubectl delete -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/2_singlepod_database.yml
 
 ================================================================================================================
 
 #Create singlepod with multicontainers
 
 create a facebook namespace first
 
 kubectl create ns facebook
 
 kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/3_singlepod_multicontianer.yml
 
[root@master-node ~]# kubectl get ns
NAME              STATUS   AGE
default           Active   33h
facebook          Active   4h32m
kube-node-lease   Active   33h
kube-public       Active   33h
kube-system       Active   33h
test              Active   94m
twitter           Active   4h32m
[root@master-node ~]# kubectl get pods -n facebook
NAME                  READY   STATUS    RESTARTS   AGE
multicontainer-pods   2/2     Running   0          76s

kubectl get all -n facebook -o wide
 
 
kubectl describe pods/multincontainer-pods -n facebook
 
# To get inside to the container
 
 to go insdie web container
 
  kubectl exec -it pod/multicontainer-pods bash -n facebook -c web
 
 to go inside database container
  kubectl exec -it pod/multicontainer-pods bash -n facebook -c db

# Delete pod multicontainer
 
 kubectl delete -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/3_singlepod_multicontianer.yml

 
 
