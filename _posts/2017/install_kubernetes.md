---
layout: default
title: Install Kubernetes on rhel7
---


# Install Kubernetes on rhel7

### Environment

| Server ip     | Role   | User |
|---------------|--------|------|
| 192.168.0.122 | master | root |
| 192.168.0.123 | node   | root |

Conditions:

* Should connect to each other successfully.
* Should access google.

### Versions
```
!341 # rpm -qa | grep kube
kubelet-1.6.2-0.x86_64
kubectl-1.6.2-0.x86_64
kubeadm-1.6.2-0.x86_64
kubernetes-cni-0.5.1-0.x86_64
```

```
!342 # docker -v
Docker version 1.12.6, build 78d1802
```


### Config Docker/Kubernetes Yum Repo

```
sudo vim /etc/yum.repos.d/docker.repo   #add the following text
 
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg

sudo vim /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
       https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg 
  
```

### Install Docker 


```
#####2. install devicemapper dependency
sudo yum install -y device-mapper-libs device-mapper-event-libs
 
 
#####3. install docker-engine
sudo yum install -y docker-engine-1.12.5
 
 
#####4. config docker service --- custom "ExecStart"
[ts1@localhost docker]$ cat /usr/lib/systemd/system/docker.service
 
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network.target


[Service]
Type=notify
##### the default is not to use systemd for cgroups because the delegate issues still
##### exists and systemd currently does not support the cgroup feature set required
##### for containers run by docker
ExecStart=/usr/bin/dockerd --live-restore -s devicemapper --storage-opt dm.loopdatasize=300G --storage-opt dm.loopmetadatasize=4G --storage-opt dm.basesize=100G --exec-opt native.cgroupdriver=cgroupfs  -g /export/docker \
         -H tcp://0.0.0.0:2375 \
         -H unix:///var/run/docker.sock \
         --insecure-registry registry.docker.dev.fwmrm.net \
         --bip 172.17.42.1/16  \
         --fixed-cidr 172.17.0.0/16
ExecReload=/bin/kill -s HUP $MAINPID
##### Having non-zero Limit*s causes performance problems due to accounting overhead
##### in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
##### Uncomment TasksMax if your systemd version supports it.
##### Only systemd 226 and above support this version.
##### TasksMax=infinity
TimeoutStartSec=0
##### set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
##### kill only the docker process, not all processes in the cgroup
KillMode=process
 
[Install]
WantedBy=multi-user.target
 
#####5. Using 'systemctl' to manage docker
sudo systemctl disable docker
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker

#####6. Config compose http timeout
export COMPOSE_HTTP_TIMEOUT=300

```

### Install Kubernetes
```
setenforce 0
yum install -y kubelet kubeadm kubectl kubernetes-cni
systemctl enable kubelet && systemctl start kubelet
```

Test kubelet separately with command

```
systemctl start kubelet
systemctl status kubelet
```

If you see these error logs in the command line,

```
● kubelet.service - kubelet: The Kubernetes Node Agent
   Loaded: loaded (/etc/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/kubelet.service.d
           └─10-kubeadm.conf
   Active: failed (Result: exit-code) since Mon 2017-05-01 07:20:03 UTC; 1s ago
     Docs: http://kubernetes.io/docs/
  Process: 40847 ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_SYSTEM_PODS_ARGS $KUBELET_DNS_ARGS $KUBELET_AUTHZ_ARGS $KUBELET_CGROUP_ARGS $KUBELET_EXTRA_ARGS (code=exited, status=1/FAILURE)
 Main PID: 40847 (code=exited, status=1/FAILURE)
```

Please open `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`, join the start command and test manually.

* For error logs, 
	- ```
error: failed to run Kubelet: failed to create kubelet: misconfiguration: kubelet cgroup driver: "systemd" is different from docker cgroup driver: "cgroupfs"
```
	please replace `--cgroup-driver=systemd` with `--cgroup-driver=cgroupfs` in 10-kubeadm.conf

	- reference: [issues/43805](https://github.com/kubernetes/kubernetes/issues/43805)


* For 
	- ```
W0501 07:12:28.505754   36144 cni.go:157] Unable to update cni config: No networks found in /etc/cni/net.d
```
	remove "KUBELET_NETWORK_ARGS" in /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

	- reference: [issues/43815](https://github.com/kubernetes/kubernetes/issues/43815)

After test manually pass, please run these commands

```
systemctl daemon-reload
systemctl start kubelet
systemctl status kubelet
systemctl stop kubelet
systemctl status kubelet
kubeadm init
```

You will see 

```
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run (as a regular user):

  sudo cp /etc/kubernetes/admin.conf $HOME/
  sudo chown $(id -u):$(id -g) $HOME/admin.conf
  export KUBECONFIG=$HOME/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  http://kubernetes.io/docs/admin/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join --token de9130.73ec7344a545d2a2 192.168.0.122:644
```

Then run command

```
kubectl get nodes
```

If you see

```
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```
Run command

```
cp /etc/kubernetes/admin.conf ~/.kube/config
```

### Add Kubernetes node
Run command on node (123):

```
kubeadm join --token de9130.73ec7344a545d2a2 192.168.0.122:644
```

Run command on master (122):

```
!352 # kubectl get nodes
NAME                       STATUS    AGE       VERSION
bjoafint01.dev.fwmrm.net   Ready     12m       v1.6.2
bjoafqa01.dev.fwmrm.net    Ready     26m       v1.6.2
```

### Deploy on Kubernetes
Use a open-source project to test

```
# create new namespace
kubectl create namespace sock-shop
# install app
kubectl apply -n sock-shop -f "https://github.com/microservices-demo/microservices-demo/blob/master/deploy/kubernetes/complete-demo.yaml?raw=true"
# check app
kubectl describe svc front-end -n sock-shop

!357 # kubectl get pods -n sock-shop
NAME                            READY     STATUS              RESTARTS   AGE
carts-153328538-lfhsp           0/1       ContainerCreating   0          1m
carts-db-4256839670-vf40p       1/1       Running             0          1m
catalogue-114596073-qmvlg       0/1       ContainerCreating   0          1m
catalogue-db-1956862931-n7n4n   0/1       ContainerCreating   0          1m
front-end-3570328172-mb909      0/1       ContainerCreating   0          1m
orders-2365168879-hjs96         0/1       ContainerCreating   0          1m
orders-db-836712666-88lq9       0/1       ContainerCreating   0          1m
payment-1968871107-t071k        0/1       ContainerCreating   0          1m
queue-master-2798459664-n8n9z   0/1       ContainerCreating   0          1m
rabbitmq-3429198581-tmf4d       0/1       ContainerCreating   0          1m
shipping-2899287913-0d548       0/1       ContainerCreating   0          1m
user-468431046-gsbjh            0/1       ContainerCreating   0          1m
user-db-1166754267-js6fq        0/1       ContainerCreating   0          1m

```

### k8s Dashboard

Run command on master

```
kubectl create -f https://rawgit.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard.yaml
kubectl describe services kubernetes-dashboard -n kube-system

Name:			kubernetes-dashboard
Namespace:		kube-system
Labels:			app=kubernetes-dashboard
Annotations:		<none>
Selector:		app=kubernetes-dashboard
Type:			NodePort
IP:			10.107.143.243
Port:			<unset>	80/TCP
NodePort:		<unset>	31824/TCP
Endpoints:		<none>
Session Affinity:	None
Events:			<none>
```

Go to http://192.168.0.122:31824

Monitor CPU and memory

```
git clone https://github.com/kubernetes/heapster.git ~/heapster
cd ~/heapster
kubectl apply -f deploy/kube-config/influxdb/
```

### Reference
https://kubernetes.io/docs/getting-started-guides/kubeadm/

https://kubernetes.io/docs/tasks/web-ui-dashboard/

https://docs.docker.com/engine/installation/linux/centos/

https://docs.docker.com/engine/admin/systemd/

https://github.com/kubernetes/dashboard

https://github.com/kubernetes/heapster/
