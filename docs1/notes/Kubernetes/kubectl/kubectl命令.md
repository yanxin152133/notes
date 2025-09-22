# 1. kubectl
## 1.1. 查询集群信息
```bash
ubuntu@master01:~$ kubectl cluster-info    ## 查看集群
Kubernetes control plane is running at https://192.168.213.135:8443
CoreDNS is running at https://192.168.213.135:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
ubuntu@master01:~$ kubectl version         ## 查看集群版本
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.0", GitCommit:"b46a3f887ca979b1a5d14fd39cb1af43e7e5d12d", GitTreeState:"clean", BuildDate:"2022-12-08T19:58:30Z", GoVersion:"go1.19.4", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.0", GitCommit:"b46a3f887ca979b1a5d14fd39cb1af43e7e5d12d", GitTreeState:"clean", BuildDate:"2022-12-08T19:51:45Z", GoVersion:"go1.19.4", Compiler:"gc", Platform:"linux/amd64"}
```

## 1.2. 查询
### 1.2.1. 显示所有namespace中的pod
```bash
ubuntu@master01:~$ kubectl get pod  -A        ## 显示所有namespace中的pod
NAMESPACE              NAME                                        READY   STATUS    RESTARTS       AGE
default                nginx-748c667d99-v9vbc                      1/1     Running   2 (31m ago)    37d
kube-flannel           kube-flannel-ds-5ktvb                       1/1     Running   5 (31m ago)    37d
kube-flannel           kube-flannel-ds-nnj4r                       1/1     Running   6 (28m ago)    37d
kube-flannel           kube-flannel-ds-nz4ls                       1/1     Running   5 (30m ago)    37d
kube-flannel           kube-flannel-ds-t7flw                       1/1     Running   6 (28m ago)    37d
kube-system            coredns-787d4945fb-497k8                    1/1     Running   3 (30m ago)    37d
kube-system            coredns-787d4945fb-wbnhl                    1/1     Running   3 (30m ago)    37d
kube-system            etcd-master01                               1/1     Running   26 (30m ago)   37d
kube-system            etcd-master02                               1/1     Running   3 (31m ago)    37d
kube-system            etcd-master03                               1/1     Running   3 (31m ago)    37d
kube-system            kube-apiserver-master01                     1/1     Running   23 (30m ago)   37d
kube-system            kube-apiserver-master02                     1/1     Running   3 (31m ago)    37d
kube-system            kube-apiserver-master03                     1/1     Running   4 (29m ago)    37d
kube-system            kube-controller-manager-master01            1/1     Running   12 (30m ago)   37d
kube-system            kube-controller-manager-master02            1/1     Running   3 (31m ago)    37d
kube-system            kube-controller-manager-master03            1/1     Running   5              37d
kube-system            kube-proxy-f4qts                            1/1     Running   3 (31m ago)    37d
kube-system            kube-proxy-h9gg6                            1/1     Running   3 (30m ago)    37d
kube-system            kube-proxy-k9mdj                            1/1     Running   3 (31m ago)    37d
kube-system            kube-proxy-zknxn                            1/1     Running   3 (31m ago)    37d
kube-system            kube-scheduler-master01                     1/1     Running   12 (30m ago)   37d
kube-system            kube-scheduler-master02                     1/1     Running   3 (31m ago)    37d
kube-system            kube-scheduler-master03                     1/1     Running   5              37d
kubernetes-dashboard   dashboard-metrics-scraper-7bc864c59-k7qrh   1/1     Running   1 (31m ago)    37d
kubernetes-dashboard   kubernetes-dashboard-58f6b9f6f8-2gqxp       1/1     Running   1 (31m ago)    37d
```

### 1.2.2. 显示所有service
```bash
ubuntu@master01:~$ kubectl get service -A          ## 显示所有service
NAMESPACE              NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
default                kubernetes                  ClusterIP   10.96.0.1        <none>        443/TCP                  37d
default                nginx                       NodePort    10.110.49.104    <none>        80:32201/TCP             37d
kube-system            kube-dns                    ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP   37d
kubernetes-dashboard   dashboard-metrics-scraper   ClusterIP   10.98.74.149     <none>        8000/TCP                 37d
kubernetes-dashboard   kubernetes-dashboard        NodePort    10.109.175.181   <none>        443:30443/TCP            37d
```

### 1.2.3. 查看默认namespace中的pod
```bash
ubuntu@master01:~$ kubectl get pod     ## 查看默认namespace中的pod
NAME                     READY   STATUS    RESTARTS      AGE
nginx-748c667d99-v9vbc   1/1     Running   2 (41m ago)   37d
```

### 1.2.4. 查看pod及更多详细信息
```bash
ubuntu@master01:~$ kubectl get pod -o wide    ## 查看pod及更多详细信息
NAME                     READY   STATUS    RESTARTS      AGE   IP            NODE     NOMINATED NODE   READINESS GATES
nginx-748c667d99-v9vbc   1/1     Running   2 (42m ago)   37d   10.244.2.26   node01   <none>           <none>
```

### 1.2.5. 以json格式输出pod及更多详细信息
```bash
ubuntu@master01:~$ kubectl get pod -o json     ## 以json格式输出pod及更多详细信息
{
    "apiVersion": "v1",
    "items": [
        {
            "apiVersion": "v1",
            "kind": "Pod",
            "metadata": {
                "creationTimestamp": "2023-01-12T20:28:36Z",
                "generateName": "nginx-748c667d99-",
                "labels": {
                    "app": "nginx",
                    "pod-template-hash": "748c667d99"
                },
                "name": "nginx-748c667d99-v9vbc",
                "namespace": "default",
                "ownerReferences": [
                    {
                        "apiVersion": "apps/v1",
                        "blockOwnerDeletion": true,
                        "controller": true,
                        "kind": "ReplicaSet",
                        "name": "nginx-748c667d99",
                        "uid": "8a4e4905-af75-48af-927f-ebd6a1248c63"
                    }
                ],
                "resourceVersion": "35281",
                "uid": "6dce0660-ccc5-4d79-8cb5-07516754e25c"
            },
            "spec": {
                "containers": [
                    {
                        "image": "nginx",
                        "imagePullPolicy": "Always",
                        "name": "nginx",
                        "resources": {},
                        "terminationMessagePath": "/dev/termination-log",
                        "terminationMessagePolicy": "File",
                        "volumeMounts": [
                            {
                                "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
                                "name": "kube-api-access-89dwb",
                                "readOnly": true
                            }
                        ]
                    }
                ],
                "dnsPolicy": "ClusterFirst",
                "enableServiceLinks": true,
                "nodeName": "node01",
                "preemptionPolicy": "PreemptLowerPriority",
                "priority": 0,
                "restartPolicy": "Always",
                "schedulerName": "default-scheduler",
                "securityContext": {},
                "serviceAccount": "default",
                "serviceAccountName": "default",
                "terminationGracePeriodSeconds": 30,
                "tolerations": [
                    {
                        "effect": "NoExecute",
                        "key": "node.kubernetes.io/not-ready",
                        "operator": "Exists",
                        "tolerationSeconds": 300
                    },
                    {
                        "effect": "NoExecute",
                        "key": "node.kubernetes.io/unreachable",
                        "operator": "Exists",
                        "tolerationSeconds": 300
                    }
                ],
                "volumes": [
                    {
                        "name": "kube-api-access-89dwb",
                        "projected": {
                            "defaultMode": 420,
                            "sources": [
                                {
                                    "serviceAccountToken": {
                                        "expirationSeconds": 3607,
                                        "path": "token"
                                    }
                                },
                                {
                                    "configMap": {
                                        "items": [
                                            {
                                                "key": "ca.crt",
                                                "path": "ca.crt"
                                            }
                                        ],
                                        "name": "kube-root-ca.crt"
                                    }
                                },
                                {
                                    "downwardAPI": {
                                        "items": [
                                            {
                                                "fieldRef": {
                                                    "apiVersion": "v1",
                                                    "fieldPath": "metadata.namespace"
                                                },
                                                "path": "namespace"
                                            }
                                        ]
                                    }
                                }
                            ]
                        }
                    }
                ]
            },
            "status": {
                "conditions": [
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2023-01-12T20:28:36Z",
                        "status": "True",
                        "type": "Initialized"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2023-02-19T15:55:57Z",
                        "status": "True",
                        "type": "Ready"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2023-02-19T15:55:57Z",
                        "status": "True",
                        "type": "ContainersReady"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2023-01-12T20:28:36Z",
                        "status": "True",
                        "type": "PodScheduled"
                    }
                ],
                "containerStatuses": [
                    {
                        "containerID": "containerd://9b0776dd780ee8b2ea73861fbf11a14d0aa1af52354b99b5ad73a55ec356d79f",
                        "image": "docker.io/library/nginx:latest",
                        "imageID": "docker.io/library/nginx@sha256:6650513efd1d27c1f8a5351cbd33edf85cc7e0d9d0fcb4ffb23d8fa89b601ba8",
                        "lastState": {
                            "terminated": {
                                "containerID": "containerd://01867ddc65da8b279d9d6a97e92ab2807935975cf6eb25e3c33ad2dbdb85a4d9",
                                "exitCode": 255,
                                "finishedAt": "2023-02-19T15:51:52Z",
                                "reason": "Unknown",
                                "startedAt": "2023-01-12T22:25:57Z"
                            }
                        },
                        "name": "nginx",
                        "ready": true,
                        "restartCount": 2,
                        "started": true,
                        "state": {
                            "running": {
                                "startedAt": "2023-02-19T15:55:57Z"
                            }
                        }
                    }
                ],
                "hostIP": "192.168.213.134",
                "phase": "Running",
                "podIP": "10.244.2.26",
                "podIPs": [
                    {
                        "ip": "10.244.2.26"
                    }
                ],
                "qosClass": "BestEffort",
                "startTime": "2023-01-12T20:28:36Z"
            }
        }
    ],
    "kind": "List",
    "metadata": {
        "resourceVersion": ""
    }
}
```

### 1.2.6. 以yaml格式输出pod及更多详细信息
```bash
ubuntu@master01:~$ kubectl get pod -o yaml    ## 以yaml格式输出pod及更多详细信息
apiVersion: v1
items:
- apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: "2023-01-12T20:28:36Z"
    generateName: nginx-748c667d99-
    labels:
      app: nginx
      pod-template-hash: 748c667d99
    name: nginx-748c667d99-v9vbc
    namespace: default
    ownerReferences:
    - apiVersion: apps/v1
      blockOwnerDeletion: true
      controller: true
      kind: ReplicaSet
      name: nginx-748c667d99
      uid: 8a4e4905-af75-48af-927f-ebd6a1248c63
    resourceVersion: "35281"
    uid: 6dce0660-ccc5-4d79-8cb5-07516754e25c
  spec:
    containers:
    - image: nginx
      imagePullPolicy: Always
      name: nginx
      resources: {}
      terminationMessagePath: /dev/termination-log
      terminationMessagePolicy: File
      volumeMounts:
      - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
        name: kube-api-access-89dwb
        readOnly: true
    dnsPolicy: ClusterFirst
    enableServiceLinks: true
    nodeName: node01
    preemptionPolicy: PreemptLowerPriority
    priority: 0
    restartPolicy: Always
    schedulerName: default-scheduler
    securityContext: {}
    serviceAccount: default
    serviceAccountName: default
    terminationGracePeriodSeconds: 30
    tolerations:
    - effect: NoExecute
      key: node.kubernetes.io/not-ready
      operator: Exists
      tolerationSeconds: 300
    - effect: NoExecute
      key: node.kubernetes.io/unreachable
      operator: Exists
      tolerationSeconds: 300
    volumes:
    - name: kube-api-access-89dwb
      projected:
        defaultMode: 420
        sources:
        - serviceAccountToken:
            expirationSeconds: 3607
            path: token
        - configMap:
            items:
            - key: ca.crt
              path: ca.crt
            name: kube-root-ca.crt
        - downwardAPI:
            items:
            - fieldRef:
                apiVersion: v1
                fieldPath: metadata.namespace
              path: namespace
  status:
    conditions:
    - lastProbeTime: null
      lastTransitionTime: "2023-01-12T20:28:36Z"
      status: "True"
      type: Initialized
    - lastProbeTime: null
      lastTransitionTime: "2023-02-19T15:55:57Z"
      status: "True"
      type: Ready
    - lastProbeTime: null
      lastTransitionTime: "2023-02-19T15:55:57Z"
      status: "True"
      type: ContainersReady
    - lastProbeTime: null
      lastTransitionTime: "2023-01-12T20:28:36Z"
      status: "True"
      type: PodScheduled
    containerStatuses:
    - containerID: containerd://9b0776dd780ee8b2ea73861fbf11a14d0aa1af52354b99b5ad73a55ec356d79f
      image: docker.io/library/nginx:latest
      imageID: docker.io/library/nginx@sha256:6650513efd1d27c1f8a5351cbd33edf85cc7e0d9d0fcb4ffb23d8fa89b601ba8
      lastState:
        terminated:
          containerID: containerd://01867ddc65da8b279d9d6a97e92ab2807935975cf6eb25e3c33ad2dbdb85a4d9
          exitCode: 255
          finishedAt: "2023-02-19T15:51:52Z"
          reason: Unknown
          startedAt: "2023-01-12T22:25:57Z"
      name: nginx
      ready: true
      restartCount: 2
      started: true
      state:
        running:
          startedAt: "2023-02-19T15:55:57Z"
    hostIP: 192.168.213.134
    phase: Running
    podIP: 10.244.2.26
    podIPs:
    - ip: 10.244.2.26
    qosClass: BestEffort
    startTime: "2023-01-12T20:28:36Z"
kind: List
metadata:
  resourceVersion: ""
```

### 1.2.7. 显示所有namespace
```bash
ubuntu@master01:~$ kubectl get namespace     ## 显示所有namespace
NAME                   STATUS   AGE
default                Active   37d
kube-flannel           Active   37d
kube-node-lease        Active   37d
kube-public            Active   37d
kube-system            Active   37d
kubernetes-dashboard   Active   37d
```

### 1.2.8. 查看namespace kube-system中的pod
```bash
ubuntu@master01:~$ kubectl get pod -n kube-system
NAME                               READY   STATUS    RESTARTS       AGE
coredns-787d4945fb-497k8           1/1     Running   3 (48m ago)    37d
coredns-787d4945fb-wbnhl           1/1     Running   3 (48m ago)    37d
etcd-master01                      1/1     Running   26 (48m ago)   37d
etcd-master02                      1/1     Running   3 (49m ago)    37d
etcd-master03                      1/1     Running   3 (49m ago)    37d
kube-apiserver-master01            1/1     Running   23 (48m ago)   37d
kube-apiserver-master02            1/1     Running   3 (49m ago)    37d
kube-apiserver-master03            1/1     Running   4 (48m ago)    37d
kube-controller-manager-master01   1/1     Running   12 (48m ago)   37d
kube-controller-manager-master02   1/1     Running   3 (49m ago)    37d
kube-controller-manager-master03   1/1     Running   5              37d
kube-proxy-f4qts                   1/1     Running   3 (50m ago)    37d
kube-proxy-h9gg6                   1/1     Running   3 (48m ago)    37d
kube-proxy-k9mdj                   1/1     Running   3 (49m ago)    37d
kube-proxy-zknxn                   1/1     Running   3 (49m ago)    37d
kube-scheduler-master01            1/1     Running   12 (48m ago)   37d
kube-scheduler-master02            1/1     Running   3 (49m ago)    37d
kube-scheduler-master03            1/1     Running   5              37d
```

## 1.3. 节点查询
### 1.3.1. 查看所有集群节点
```bash
ubuntu@master01:~$ kubectl get nodes    ## 查看所有集群节点
NAME       STATUS   ROLES           AGE   VERSION
master01   Ready    control-plane   37d   v1.26.0
master02   Ready    control-plane   37d   v1.26.0
master03   Ready    control-plane   37d   v1.26.0
node01     Ready    <none>          37d   v1.26.0
```

### 1.3.2. 查看集群节点及更多信息
```bash
ubuntu@master01:~$ kubectl get nodes -o wide
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP       EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
master01   Ready    control-plane   37d   v1.26.0   192.168.213.131   <none>        Ubuntu 20.04.5 LTS   5.4.0-136-generic   containerd://1.6.18
master02   Ready    control-plane   37d   v1.26.0   192.168.213.132   <none>        Ubuntu 20.04.5 LTS   5.4.0-136-generic   containerd://1.6.18
master03   Ready    control-plane   37d   v1.26.0   192.168.213.133   <none>        Ubuntu 20.04.5 LTS   5.4.0-136-generic   containerd://1.6.18
node01     Ready    <none>          37d   v1.26.0   192.168.213.134   <none>        Ubuntu 20.04.5 LTS   5.4.0-136-generic   containerd://1.6.18
```

### 1.3.3. 显示node详细信息
```bash
ubuntu@master01:~$ kubectl describe node node01    ## 显示node详细信息
Name:               node01
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=node01
                    kubernetes.io/os=linux
Annotations:        flannel.alpha.coreos.com/backend-data: null
                    flannel.alpha.coreos.com/backend-type: host-gw
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 192.168.213.134
                    kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Thu, 12 Jan 2023 19:58:52 +0000
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  node01
  AcquireTime:     <unset>
  RenewTime:       Sun, 19 Feb 2023 16:47:08 +0000
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Sun, 19 Feb 2023 15:55:40 +0000   Sun, 19 Feb 2023 15:55:40 +0000   FlannelIsUp                  Flannel is running on this node
  MemoryPressure       False   Sun, 19 Feb 2023 16:45:29 +0000   Thu, 12 Jan 2023 19:58:52 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Sun, 19 Feb 2023 16:45:29 +0000   Thu, 12 Jan 2023 19:58:52 +0000   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Sun, 19 Feb 2023 16:45:29 +0000   Thu, 12 Jan 2023 19:58:52 +0000   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Sun, 19 Feb 2023 16:45:29 +0000   Thu, 12 Jan 2023 20:03:59 +0000   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  InternalIP:  192.168.213.134
  Hostname:    node01
Capacity:
  cpu:                2
  ephemeral-storage:  20463184Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             1999368Ki
  pods:               110
Allocatable:
  cpu:                2
  ephemeral-storage:  18858870344
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             1896968Ki
  pods:               110
System Info:
  Machine ID:                 cce1668c8f91492c8c8452b565a8c54c
  System UUID:                0b174d56-521e-ff18-840c-78e01d7d4175
  Boot ID:                    d814e4cb-5cc3-4e24-bc66-6f98acdaf764
  Kernel Version:             5.4.0-136-generic
  OS Image:                   Ubuntu 20.04.5 LTS
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.6.18
  Kubelet Version:            v1.26.0
  Kube-Proxy Version:         v1.26.0
PodCIDR:                      10.244.2.0/24
PodCIDRs:                     10.244.2.0/24
Non-terminated Pods:          (5 in total)
  Namespace                   Name                                         CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                         ------------  ----------  ---------------  -------------  ---
  default                     nginx-748c667d99-v9vbc                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         37d
  kube-flannel                kube-flannel-ds-t7flw                        100m (5%)     0 (0%)      50Mi (2%)        0 (0%)         37d
  kube-system                 kube-proxy-f4qts                             0 (0%)        0 (0%)      0 (0%)           0 (0%)         37d
  kubernetes-dashboard        dashboard-metrics-scraper-7bc864c59-k7qrh    0 (0%)        0 (0%)      0 (0%)           0 (0%)         37d
  kubernetes-dashboard        kubernetes-dashboard-58f6b9f6f8-2gqxp        0 (0%)        0 (0%)      0 (0%)           0 (0%)         37d
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests   Limits
  --------           --------   ------
  cpu                100m (5%)  0 (0%)
  memory             50Mi (2%)  0 (0%)
  ephemeral-storage  0 (0%)     0 (0%)
  hugepages-1Gi      0 (0%)     0 (0%)
  hugepages-2Mi      0 (0%)     0 (0%)
Events:
  Type     Reason                   Age                From             Message
  ----     ------                   ----               ----             -------
  Normal   Starting                 52m                kube-proxy       
  Warning  InvalidDiskCapacity      54m                kubelet          invalid capacity 0 on image filesystem
  Normal   NodeAllocatableEnforced  54m                kubelet          Updated Node Allocatable limit across pods
  Normal   NodeHasNoDiskPressure    54m (x7 over 54m)  kubelet          Node node01 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     54m (x7 over 54m)  kubelet          Node node01 status is now: NodeHasSufficientPID
  Normal   NodeHasSufficientMemory  54m (x8 over 54m)  kubelet          Node node01 status is now: NodeHasSufficientMemory
  Normal   RegisteredNode           52m                node-controller  Node node01 event: Registered Node node01 in Controller
  Normal   RegisteredNode           41m                node-controller  Node node01 event: Registered Node node01 in Controller
  Warning  ContainerGCFailed        32m                kubelet          rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing dial unix /var/run/containerd/containerd.sock: connect: no such file or directory"
```

### 1.3.4. 查看pod详细信息
```bash
ubuntu@master01:~$ kubectl describe pod nginx-748c667d99-v9vbc -n default    ## 查看pod详细信息   nginx-748c667d99-v9vbc 为kubectl get pod -A 命令输出结果中的nginx-748c667d99-v9vbc
Name:             nginx-748c667d99-v9vbc
Namespace:        default
Priority:         0
Service Account:  default
Node:             node01/192.168.213.134
Start Time:       Thu, 12 Jan 2023 20:28:36 +0000
Labels:           app=nginx
                  pod-template-hash=748c667d99
Annotations:      <none>
Status:           Running
IP:               10.244.2.26
IPs:
  IP:           10.244.2.26
Controlled By:  ReplicaSet/nginx-748c667d99
Containers:
  nginx:
    Container ID:   containerd://9b0776dd780ee8b2ea73861fbf11a14d0aa1af52354b99b5ad73a55ec356d79f
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:6650513efd1d27c1f8a5351cbd33edf85cc7e0d9d0fcb4ffb23d8fa89b601ba8
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sun, 19 Feb 2023 15:55:57 +0000
    Last State:     Terminated
      Reason:       Unknown
      Exit Code:    255
      Started:      Thu, 12 Jan 2023 22:25:57 +0000
      Finished:     Sun, 19 Feb 2023 15:51:52 +0000
    Ready:          True
    Restart Count:  2
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-89dwb (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-89dwb:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason                  Age                From     Message
  ----     ------                  ----               ----     -------
  Warning  FailedCreatePodSandBox  55m                kubelet  Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "57ba08a1e4a619d923137901225894cf729dcb24902d03d1766b7ab86b07ed1d": plugin type="flannel" failed (add): loadFlannelSubnetEnv failed: open /run/flannel/subnet.env: no such file or directory
  Warning  FailedCreatePodSandBox  54m                kubelet  Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "6db852a66c7e49cb0f045f150e2f026703689c4aaabf5e7a145a9b43cc519850": plugin type="flannel" failed (add): loadFlannelSubnetEnv failed: open /run/flannel/subnet.env: no such file or directory
  Warning  FailedCreatePodSandBox  54m                kubelet  Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "12a47b401f31ab1320c65023c57eb8c151560a36a75a9faf36d1804064db02b0": plugin type="flannel" failed (add): loadFlannelSubnetEnv failed: open /run/flannel/subnet.env: no such file or directory
  Warning  FailedCreatePodSandBox  54m                kubelet  Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "d9f304805744192c63bee8fbd8ed20bee7c98bab750a2988224f7d4e2b26ccce": plugin type="flannel" failed (add): loadFlannelSubnetEnv failed: open /run/flannel/subnet.env: no such file or directory
  Warning  FailedCreatePodSandBox  54m                kubelet  Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "10a8bc77ab8e7a8e74486783e4e5389093378be87bf86cfee1a2e2e22f7c796b": plugin type="flannel" failed (add): loadFlannelSubnetEnv failed: open /run/flannel/subnet.env: no such file or directory
  Normal   SandboxChanged          54m (x6 over 55m)  kubelet  Pod sandbox changed, it will be killed and re-created.
  Normal   Pulling                 54m                kubelet  Pulling image "nginx"
  Normal   Pulled                  53m                kubelet  Successfully pulled image "nginx" in 13.945107049s (13.945172432s including waiting)
  Normal   Created                 53m                kubelet  Created container nginx
  Normal   Started                 53m                kubelet  Started container nginx
```

### 1.3.5. 进入pod
```bash
ubuntu@master01:~$ kubectl exec -it nginx-748c667d99-v9vbc -n default bash   ## 进入pod
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
root@nginx-748c667d99-v9vbc:/#  
```

### 1.3.6. 查看pod运行日志
```bash
ubuntu@master01:~$ kubectl logs nginx-748c667d99-v9vbc -n default
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/02/19 15:55:57 [notice] 1#1: using the "epoll" event method
2023/02/19 15:55:57 [notice] 1#1: nginx/1.23.3
2023/02/19 15:55:57 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/02/19 15:55:57 [notice] 1#1: OS: Linux 5.4.0-136-generic
2023/02/19 15:55:57 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/02/19 15:55:57 [notice] 1#1: start worker processes
2023/02/19 15:55:57 [notice] 1#1: start worker process 29
2023/02/19 15:55:57 [notice] 1#1: start worker process 30
```

## 1.4. 创建资源
```bash
kubectl create -f namespace.yaml
kubectl apply -f namespace.yaml
```

## 1.5. 删除资源
```bash
kubectl get deployments
kubectl delete deployments c1
kubectl delete pod c1
kubectl delete -f namespcae.yaml
```

## 1.6. 动态修改资源
```bash
kubectl edit deployment nginx-deploment   #动态修改控制器
kubectl scale deployment my-dp --replicas=4   #修改副本数为2
kubectl  rollout restart deploy account-uc-console -n jd-tpaas  #重启deploy控制器
```

## 1.7. 标签相关
```bash
kubectl get nodes --show-labels  #查看集群节点以及lables信息
kubectl label nodes node1 name=ssy    #为node添加label标签
kubectl label nodes node1 name=ss1 --overwrite   #修改node节点的标签
kubectl label nodes node1 name-    #删除node标签
```

## 1.8. 调度
```bash
kubectl cordon master禁止节点调度
kubeclt uncordon master 允许节点调度
```