
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

 -------------------------------------------------------------------
 
 Single Pod with multicontainer with duplicate ports
 
 4_singlepod_duplicatePort.yml
 
 
 
 kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/4_singlepod_duplicateport.yml
Error from server (BadRequest): error when creating "https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/4_singlepod_duplicateport.yml": pod in version "v1" cannot be handled as a Pod: no kind "pod" is registered for version "v1" in scheme "k8s.io/kubernetes/pkg/api/legacyscheme/scheme.go:30"
 
The Pod "multicn_duplicate-ports" is invalid: metadata.name: Invalid value: "multicn_duplicate-ports": a DNS-1123 subdomain must consist of lower case alphanumeric characters, '-' or '.', and must start and end with an alphanumeric character (e.g. 'example.com', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?(\.[a-z0-9]([-a-z0-9]*[a-z0-9])?)*')
 
 solution: metadata name changed
 
kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/4_singlepod_duplicateport.yml 

 
 finally pod created
 root@master-node ~]# kubectl get pods -n facebook
NAME                 READY   STATUS             RESTARTS   AGE
duplicateport-pods   1/2     CrashLoopBackOff   2          48s

 
 [root@master-node ~]# kubectl describe pod duplicateport-pods -n facebook
Name:         duplicateport-pods
Namespace:    facebook
Priority:     0
Node:         worker-1/192.168.122.230
Start Time:   Tue, 13 Jul 2021 19:37:16 -0400
Labels:       app=httpd
              tier=frontend-backend
              version=v1
Annotations:  cni.projectcalico.org/podIP: 192.168.226.69/32
              cni.projectcalico.org/podIPs: 192.168.226.69/32
Status:       Running
IP:           192.168.226.69
IPs:
  IP:  192.168.226.69
Containers:
  httpd1:
    Container ID:   docker://782f417116283c01fff9a5cfb766ac2e0e8cc52c5ddaa4c65089b2bc49dbf606
    Image:          httpd
    Image ID:       docker-pullable://httpd@sha256:1fd07d599a519b594b756d2e4e43a72edf7e30542ce646f5eb3328cf3b12341a
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 13 Jul 2021 19:37:17 -0400
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-h6bfb (ro)
  httpd2:
    Container ID:   docker://5ea390b6a9e1c9ef049f645a30e87ce915acedf80662e79fd61933ca507c537e
    Image:          httpd
    Image ID:       docker-pullable://httpd@sha256:1fd07d599a519b594b756d2e4e43a72edf7e30542ce646f5eb3328cf3b12341a
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Tue, 13 Jul 2021 19:38:58 -0400
      Finished:     Tue, 13 Jul 2021 19:38:58 -0400
    Ready:          False
    Restart Count:  4
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-h6bfb (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  default-token-h6bfb:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-h6bfb
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason     Age                 From               Message
  ----     ------     ----                ----               -------
  Normal   Scheduled  <unknown>           default-scheduler  Successfully assigned facebook/duplicateport-pods to worker-1
  Normal   Pulling    2m1s                kubelet, worker-1  Pulling image "httpd"
  Normal   Pulled     2m                  kubelet, worker-1  Successfully pulled image "httpd"
  Normal   Created    2m                  kubelet, worker-1  Created container httpd1
  Normal   Started    119s                kubelet, worker-1  Started container httpd1
  Normal   Pulling    71s (x4 over 119s)  kubelet, worker-1  Pulling image "httpd"
  Normal   Pulled     70s (x4 over 119s)  kubelet, worker-1  Successfully pulled image "httpd"
  Normal   Created    70s (x4 over 118s)  kubelet, worker-1  Created container httpd2
  Normal   Started    70s (x4 over 118s)  kubelet, worker-1  Started container httpd2
  Warning  BackOff    70s (x5 over 116s)  kubelet, worker-1  Back-off restarting failed container
[root@master-node ~]# kubectl get all -n facebook -o wide
NAME                     READY   STATUS             RESTARTS   AGE     IP               NODE       NOMINATED NODE   READINESS GATES
pod/duplicateport-pods   1/2     CrashLoopBackOff   4          3m12s   192.168.226.69   worker-1   <none>           <none>

 
Go to inside container and check the details
 
one contianer will run and second will give an error

kubectl logs pod/multicontainer-pods -n facebook -c httpd2

 verify log below: error container- not runnung
 ubectl logs pod/duplicateport-pods -n facebook -c httpd2
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 192.168.226.69. Set the 'ServerName' directive globally to suppress this message
(98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:80
no listening sockets available, shutting down
AH00015: Unable to open logs

  
 
kubectl logs pod/multicontainer-pods -n twitter -c httpd1

 verify log below: - running container
 
 [root@master-node ~]# kubectl logs pod/duplicateport-pods -n facebook -c httpd1
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 192.168.226.69. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 192.168.226.69. Set the 'ServerName' directive globally to suppress this message
[Tue Jul 13 23:37:18.025990 2021] [mpm_event:notice] [pid 1:tid 139719283500160] AH00489: Apache/2.4.48 (Unix) configured -- resuming normal operations
[Tue Jul 13 23:37:18.026070 2021] [core:notice] [pid 1:tid 139719283500160] AH00094: Command line: 'httpd -D FOREGROUND'

 
 

 
 
 
