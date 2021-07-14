storage

kubectl apply -f https://raw.githubusercontent.com/prafulpatel16/kubernetes-homelab/master/pods/5_storage.yml

root@master-node ~]# kubectl describe pod database -n twitter
Name:         database
Namespace:    twitter
Priority:     0
Node:         worker-1/192.168.122.230
Start Time:   Tue, 13 Jul 2021 20:34:58 -0400
Labels:       app=psql
              tier=backend
              version=master
Annotations:  cni.projectcalico.org/podIP: 192.168.226.70/32
              cni.projectcalico.org/podIPs: 192.168.226.70/32
Status:       Running
IP:           192.168.226.70
IPs:
  IP:  192.168.226.70
Containers:
  psql:
    Container ID:   docker://ed9d4e277c2b0e70c526176e0bd578adbecf6039c2e3b714c5491d53f318a22c
    Image:          postgres
    Image ID:       docker-pullable://postgres@sha256:2b87b5bb55589540f598df6ec5855e5c15dd13628230a689d46492c1d433c4df
    Port:           5432/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 13 Jul 2021 20:35:20 -0400
    Ready:          True
    Restart Count:  0
    Environment:
      POSTGRES_PASSWORD:  Password@123
    Mounts:
      /var/lib/postgresql/data from pgdata (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8gvgh (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  pgdata:
    Type:          HostPath (bare host directory volume)
    Path:          /var/lib/postgres
    HostPathType:  DirectoryOrCreate
  default-token-8gvgh:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-8gvgh
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age        From               Message
  ----    ------     ----       ----               -------
  Normal  Scheduled  <unknown>  default-scheduler  Successfully assigned twitter/database to worker-1
  Normal  Pulling    5m33s      kubelet, worker-1  Pulling image "postgres"
  Normal  Pulled     5m12s      kubelet, worker-1  Successfully pulled image "postgres"
  Normal  Created    5m12s      kubelet, worker-1  Created container psql
  Normal  Started    5m11s      kubelet, worker-1  Started container psql
  
  
  
  Go to worker-1 where the pod is deployed and check the storage is mounted
  
  on this path Path:          /var/lib/postgres
  
  ls -ltrh Path:          /var/lib/postgres
  
 [root@worker-1 ~]# ls -ltrh /var/lib/postgres
total 60K
-rw------- 1 systemd-coredump input    3 Jul 13 18:35 PG_VERSION
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_twophase
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_tblspc
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_snapshots
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_serial
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_replslot
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_notify
drwx------ 4 systemd-coredump input   36 Jul 13 18:35 pg_multixact
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_dynshmem
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_commit_ts
-rw------- 1 systemd-coredump input  28K Jul 13 18:35 postgresql.conf
-rw------- 1 systemd-coredump input   88 Jul 13 18:35 postgresql.auto.conf
-rw------- 1 systemd-coredump input 1.6K Jul 13 18:35 pg_ident.conf
drwx------ 2 systemd-coredump input   18 Jul 13 18:35 pg_xact
drwx------ 3 systemd-coredump input   60 Jul 13 18:35 pg_wal
drwx------ 2 systemd-coredump input   18 Jul 13 18:35 pg_subtrans
drwx------ 5 systemd-coredump input   41 Jul 13 18:35 base
-rw------- 1 systemd-coredump input 4.7K Jul 13 18:35 pg_hba.conf
-rw------- 1 systemd-coredump input   36 Jul 13 18:35 postmaster.opts
-rw------- 1 systemd-coredump input   94 Jul 13 18:35 postmaster.pid
drwx------ 2 systemd-coredump input    6 Jul 13 18:35 pg_stat
drwx------ 2 systemd-coredump input 4.0K Jul 13 18:36 global
drwx------ 4 systemd-coredump input   68 Jul 13 18:40 pg_logical
drwx------ 2 systemd-coredump input   63 Jul 13 18:44 pg_stat_tmp
