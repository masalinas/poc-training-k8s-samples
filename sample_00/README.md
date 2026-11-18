# Description
Execute some kubernetes commands to check the architecture and principal resources and services deployed in your cluster.

## Kubernetes commands
- Get kubernetes version
```shell
$ kubectl version
Client Version: v1.31.0
Kustomize Version: v5.4.2
Server Version: v1.31.14
```

- Get kubernetes component status
```shell
$ kubectl get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE   ERROR
controller-manager   Healthy   ok        
scheduler            Healthy   ok        
etcd-0               Healthy   ok 
```

Now is recommended execute this other command
```shell
$ kubectl get pods -n kube-system
NAME                                 READY   STATUS    RESTARTS        AGE
coredns-7c65d6cfc9-4pn56             1/1     Running   7 (8m40s ago)   3d13h
coredns-7c65d6cfc9-wqd6m             1/1     Running   7 (8m40s ago)   3d13h
etcd-k8s-master                      1/1     Running   7 (8m40s ago)   3d13h
kube-apiserver-k8s-master            1/1     Running   7 (8m40s ago)   3d13h
kube-controller-manager-k8s-master   1/1     Running   7 (8m40s ago)   3d13h
kube-proxy-584sq                     1/1     Running   5 (8m24s ago)   3d12h
kube-proxy-n44zz                     1/1     Running   7 (8m40s ago)   3d13h
kube-scheduler-k8s-master            1/1     Running   7 (8m40s ago)   3d13h
```

- Get all nodes of the cluster
```shell
$ kubectl get nodes
NAME          STATUS   ROLES           AGE     VERSION
k8s-master    Ready    control-plane   3d13h   v1.31.14
k8s-worker1   Ready    <none>          3d12h   v1.31.14
```

- Describe a master node of your cluster
```shell
$ kubectl describe nodes k8s-master
Name:               k8s-master
Roles:              control-plane
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=k8s-master
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node.kubernetes.io/exclude-from-external-load-balancers=
Annotations:        flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"fe:c0:ed:4c:35:f4"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 192.168.56.10
                    kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 14 Nov 2026 22:49:11 +0100
Taints:             node-role.kubernetes.io/control-plane:NoSchedule
Unschedulable:      false
Lease:
  HolderIdentity:  k8s-master
  AcquireTime:     <unset>
  RenewTime:       Wed, 18 Nov 2026 12:02:39 +0100
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Wed, 18 Nov 2026 11:52:00 +0100   Wed, 18 Nov 2026 11:52:00 +0100   FlannelIsUp                  Flannel is running on this node
  MemoryPressure       False   Wed, 18 Nov 2026 12:02:07 +0100   Sat, 14 Nov 2026 22:49:10 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Wed, 18 Nov 2026 12:02:07 +0100   Sat, 14 Nov 2026 22:49:10 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Wed, 18 Nov 2026 12:02:07 +0100   Sat, 14 Nov 2026 22:49:10 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Wed, 18 Nov 2026 12:02:07 +0100   Sat, 14 Nov 2026 23:10:08 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.56.10
  Hostname:    k8s-master
Capacity:
  cpu:                2
  ephemeral-storage:  11758760Ki
  hugepages-2Mi:      0
  memory:             3039888Ki
  pods:               110
Allocatable:
  cpu:                2
  ephemeral-storage:  10836873199
  hugepages-2Mi:      0
  memory:             2937488Ki
  pods:               110
System Info:
  Machine ID:                 88a2fda00e2c4c93a0c2243fbf1aa688
  System UUID:                a3473def-7842-714a-9d46-dc1d88c68cf4
  Boot ID:                    c5c5a979-7fa3-4b87-9707-f70f57d82280
  Kernel Version:             5.15.0-203-generic
  OS Image:                   Ubuntu 22.04.5 LTS
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://2.2.1
  Kubelet Version:            v1.31.14
  Kube-Proxy Version:         v1.31.14
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
Non-terminated Pods:          (8 in total)
  Namespace                   Name                                  CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                  ------------  ----------  ---------------  -------------  ---
  kube-flannel                kube-flannel-ds-wfnq6                 100m (5%)     0 (0%)      50Mi (1%)        0 (0%)         3d12h
  kube-system                 coredns-7c65d6cfc9-4pn56              100m (5%)     0 (0%)      70Mi (2%)        170Mi (5%)     3d13h
  kube-system                 coredns-7c65d6cfc9-wqd6m              100m (5%)     0 (0%)      70Mi (2%)        170Mi (5%)     3d13h
  kube-system                 etcd-k8s-master                       100m (5%)     0 (0%)      100Mi (3%)       0 (0%)         3d13h
  kube-system                 kube-apiserver-k8s-master             250m (12%)    0 (0%)      0 (0%)           0 (0%)         3d13h
  kube-system                 kube-controller-manager-k8s-master    200m (10%)    0 (0%)      0 (0%)           0 (0%)         3d13h
  kube-system                 kube-proxy-n44zz                      0 (0%)        0 (0%)      0 (0%)           0 (0%)         3d13h
  kube-system                 kube-scheduler-k8s-master             100m (5%)     0 (0%)      0 (0%)           0 (0%)         3d13h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests     Limits
  --------           --------     ------
  cpu                950m (47%)   0 (0%)
  memory             290Mi (10%)  340Mi (11%)
  ephemeral-storage  0 (0%)       0 (0%)
  hugepages-2Mi      0 (0%)       0 (0%)
Events:
  Type     Reason                   Age                    From             Message
  ----     ------                   ----                   ----             -------
  Normal   Starting                 3h16m                  kube-proxy       
  Normal   Starting                 10m                    kube-proxy       
  Normal   NodeAllocatableEnforced  3h16m                  kubelet          Updated Node Allocatable limit across pods
  Normal   NodeHasSufficientMemory  3h16m (x8 over 3h16m)  kubelet          Node k8s-master status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    3h16m (x8 over 3h16m)  kubelet          Node k8s-master status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     3h16m (x7 over 3h16m)  kubelet          Node k8s-master status is now: NodeHasSufficientPID
  Normal   Starting                 3h16m                  kubelet          Starting kubelet.
  Normal   RegisteredNode           3h16m                  node-controller  Node k8s-master event: Registered Node k8s-master in Controller
  Normal   Starting                 10m                    kubelet          Starting kubelet.
  Warning  InvalidDiskCapacity      10m                    kubelet          invalid capacity 0 on image filesystem
  Normal   NodeHasSufficientMemory  10m (x8 over 10m)      kubelet          Node k8s-master status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    10m (x7 over 10m)      kubelet          Node k8s-master status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     10m (x7 over 10m)      kubelet          Node k8s-master status is now: NodeHasSufficientPID
  Normal   NodeAllocatableEnforced  10m                    kubelet          Updated Node Allocatable limit across pods
  Normal   RegisteredNode           10m                    node-controller  Node k8s-master event: Registered Node k8s-master in Controller
```

- Describe a worker node of your cluster
```shell
$ kubectl describe nodes k8s-worker1
Name:               k8s-worker1
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=k8s-worker1
                    kubernetes.io/os=linux
Annotations:        flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"86:59:05:0b:ca:1f"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 192.168.56.11
                    kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 14 Nov 2026 23:17:36 +0100
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  k8s-worker1
  AcquireTime:     <unset>
  RenewTime:       Wed, 18 Nov 2026 12:04:00 +0100
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Wed, 18 Nov 2026 11:52:10 +0100   Wed, 18 Nov 2026 11:52:10 +0100   FlannelIsUp                  Flannel is running on this node
  MemoryPressure       False   Wed, 18 Nov 2026 12:02:17 +0100   Sun, 15 Nov 2026 20:44:09 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Wed, 18 Nov 2026 12:02:17 +0100   Sun, 15 Nov 2026 20:44:09 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Wed, 18 Nov 2026 12:02:17 +0100   Sun, 15 Nov 2026 20:44:09 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Wed, 18 Nov 2026 12:02:17 +0100   Sun, 15 Nov 2026 20:44:09 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.56.11
  Hostname:    k8s-worker1
Capacity:
  cpu:                2
  ephemeral-storage:  10218772Ki
  hugepages-2Mi:      0
  memory:             2010772Ki
  pods:               110
Allocatable:
  cpu:                2
  ephemeral-storage:  9417620260
  hugepages-2Mi:      0
  memory:             1908372Ki
  pods:               110
System Info:
  Machine ID:                 a75839308f6d4c198bb8c8281fef8df0
  System UUID:                2c0c6db0-a67f-f44f-b769-b6f1a1a51d2c
  Boot ID:                    e2db4a91-c0e9-496d-9397-ffad5c631d0a
  Kernel Version:             5.15.0-203-generic
  OS Image:                   Ubuntu 22.04.5 LTS
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://2.2.1
  Kubelet Version:            v1.31.14
  Kube-Proxy Version:         v1.31.14
PodCIDR:                      10.244.1.0/24
PodCIDRs:                     10.244.1.0/24
Non-terminated Pods:          (5 in total)
  Namespace                   Name                                          CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                          ------------  ----------  ---------------  -------------  ---
  default                     nginx-test-5b77bfd686-xsbgg                   0 (0%)        0 (0%)      0 (0%)           0 (0%)         2d2h
  kube-flannel                kube-flannel-ds-hqknh                         100m (5%)     0 (0%)      50Mi (2%)        0 (0%)         3d12h
  kube-system                 kube-proxy-584sq                              0 (0%)        0 (0%)      0 (0%)           0 (0%)         3d12h
  kubernetes-dashboard        dashboard-metrics-scraper-6b96ff7878-2xhlz    0 (0%)        0 (0%)      0 (0%)           0 (0%)         46h
  kubernetes-dashboard        kubernetes-dashboard-8696f5f494-hxd9c         0 (0%)        0 (0%)      0 (0%)           0 (0%)         46h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests   Limits
  --------           --------   ------
  cpu                100m (5%)  0 (0%)
  memory             50Mi (2%)  0 (0%)
  ephemeral-storage  0 (0%)     0 (0%)
  hugepages-2Mi      0 (0%)     0 (0%)
Events:
  Type     Reason                   Age    From             Message
  ----     ------                   ----   ----             -------
  Normal   Starting                 3h17m  kube-proxy       
  Normal   Starting                 12m    kube-proxy       
  Normal   Starting                 3h17m  kubelet          Starting kubelet.
  Warning  InvalidDiskCapacity      3h17m  kubelet          invalid capacity 0 on image filesystem
  Normal   NodeAllocatableEnforced  3h17m  kubelet          Updated Node Allocatable limit across pods
  Warning  Rebooted                 3h17m  kubelet          Node k8s-worker1 has been rebooted, boot id: 1b1379be-6471-4b2f-9365-188153323cbe
  Normal   NodeHasNoDiskPressure    3h17m  kubelet          Node k8s-worker1 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     3h17m  kubelet          Node k8s-worker1 status is now: NodeHasSufficientPID
  Normal   NodeHasSufficientMemory  3h17m  kubelet          Node k8s-worker1 status is now: NodeHasSufficientMemory
  Normal   RegisteredNode           3h17m  node-controller  Node k8s-worker1 event: Registered Node k8s-worker1 in Controller
  Normal   Starting                 12m    kubelet          Starting kubelet.
  Warning  InvalidDiskCapacity      12m    kubelet          invalid capacity 0 on image filesystem
  Normal   NodeAllocatableEnforced  12m    kubelet          Updated Node Allocatable limit across pods
  Normal   NodeHasSufficientMemory  12m    kubelet          Node k8s-worker1 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    12m    kubelet          Node k8s-worker1 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     12m    kubelet          Node k8s-worker1 status is now: NodeHasSufficientPID
  Warning  Rebooted                 12m    kubelet          Node k8s-worker1 has been rebooted, boot id: e2db4a91-c0e9-496d-9397-ffad5c631d0a
  Normal   RegisteredNode           11m    node-controller  Node k8s-worker1 event: Registered Node k8s-worker1 in Controller
```

- Describe the kubernetes proxy service
```shell
$ kubectl get daemonSets --namespace=kube-system kube-proxy
NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-proxy   2         2         2       2            2           kubernetes.io/os=linux   3d13h
```

- Describe the kubernetes DNS service
```shell
$ kubectl get deployments --namespace=kube-system coredns
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
coredns   2/2     2            2           3d13h
```

- Get all  namepsaces fo your cluster
```shell
$ kubectl get namespaces
NAME                   STATUS   AGE
default                Active   3d13h
kube-flannel           Active   3d13h
kube-node-lease        Active   3d13h
kube-public            Active   3d13h
kube-system            Active   3d13h
kubernetes-dashboard   Active   46hkubectl get namespaces
```

- Get all resources from Kubernetes Dashboard service
```shell
$ kubectl get all -n kubernetes-dashboard
NAME                                             READY   STATUS    RESTARTS      AGE
pod/dashboard-metrics-scraper-6b96ff7878-2xhlz   1/1     Running   2 (18m ago)   46h
pod/kubernetes-dashboard-8696f5f494-hxd9c        1/1     Running   2 (18m ago)   46h

NAME                                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/dashboard-metrics-scraper   ClusterIP   10.102.163.161   <none>        8000/TCP   46h
service/kubernetes-dashboard        ClusterIP   10.99.2.27       <none>        443/TCP    46h

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dashboard-metrics-scraper   1/1     1            1           46h
deployment.apps/kubernetes-dashboard        1/1     1            1           46h

NAME                                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/dashboard-metrics-scraper-6b96ff7878   1         1         1       46h
replicaset.apps/kubernetes-dashboard-8696f5f494        1         1         1       46h
```
