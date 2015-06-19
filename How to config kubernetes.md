## Environment request
* 1 vm for master 
* 2 vms for minons
* vms should at least 30GB free disk  

## Install Kurbernetes
1. enable repos if you have not done before:

   `subscription-manager repos --enalbe=rhel-7-server-extras-rpms`

   `subscription-manager repos --enable=rhel-7-server-optional-rpms`
2. Install following rpms on all vms:

   `yum install -y kubernetes docker etcd `

## Config Master Node
* There are three files you need to config in master node:

  `/etc/kubernetes/config`

  `/etc/kubernetes/controller-manager`

  `/etc/kubernetes/apiserver`

* Config `/etc/kubernetes/config`:

`KUBE_LOGTOSTDERR="--logtostderr=true"`

`KUBE_LOG_LEVEL="--v=0"`

`KUBE_ALLOW_PRIV="--allow_privileged=false"`

`KUBE_MASTER="--master=http://Master.example.com:8080"`

* Config `/etc/kubernetes/controller-manager`

`KUBELET_ADDRESSES="--machines=Minion1.example.com, Minion2.example.com"`

`KUBE_CONTROLLER_MANAGER_ARGS=""`

* Config `/etc/kubernetes/apiserver`

`KUBE_API_ADDRESS="--address=0.0.0.0"`

`KUBE_ETCD_SERVERS="--etcd_servers=http://Master.example.com"`

`KUBE_SERVICE_ADDRESSES="--portal_net=10.254.0.0/16"`

`KUBE_ADMISSION_CONTROL="--admission_control=NamespaceAutoProvision,LimitRanger,ResourceQuota"`

`KUBE_API_ARGS=""`

##Config Minion Nodes
* There are four files you need to config in every minion node:

  `/etc/kubernetes/config`

  `/etc/kubernetes/proxy`

  `/etc/kubernetes/kubelet`

  `/var/lib/kubelet/auth`

* Config `/etc/kubernetes/config`

`KUBE_LOGTOSTDERR="--logtostderr=true"`

`KUBE_LOG_LEVEL="--v=0"`

`KUBE_ALLOW_PRIV="--allow_privileged=false"`

`KUBE_MASTER="--master=http://Master.example.com:8080"`

* Config `/etc/kubernetes/proxy`

`KUBE_PROXY_ARGS="--master=http://Master.example.com"`

* Config `/etc/kubernetes/kubelet`

`KUBELET_ADDRESS="--address=0.0.0.0"`

`KUBELET_HOSTNAME="--hostname_override=Minion.example.com"`

`KUBELET_API_SERVER="--api_servers=http://Master.example.com:8080"`

`KUBELET_ARGS=""`

* Config  `/var/lib/kubelet/auth`

 `echo {} > /var/lib/kubelet/auth`

## Starting and enable services in Master node
`for SERVICES in etcd kube-apiserver kube-controller-manager kube-scheduler; do systemctl restart $SERVICES; systemctl enable $SERVICES; systemctl status $SERVICES; done`

## Starting and enable services in Minion node
`for SERVICES in docker kube-proxy kubelet; do systemctl restart $SERVICES; systemctl enable $SERVICES; systemctl status $SERVICES; done`

## Confirm if your environment is good to go:

* In master node, run following command:

`Kubectl get minions`

* If everything is good, you should be able to see following return means that your minion is ready:

`NAME                 LABELS                STATUS`

`Minion1.example.com   Schedulable   <none>    Ready`

`Minion2.example.com   Schedulable   <none>    Ready`




