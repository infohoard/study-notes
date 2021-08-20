# Certified Kubernetes Application Developer Study Notes


## Basic Node Maintainance
<details>
  <summary>The Saikang you need to do for your nodes</summary>
  
  ### Backing up of ETCD
  ETCD acts as the 'database' of your cluster.  <br/>
  Drop etcd = drop cluster.
  
  Your ETCD configuration is in `/etc/kubernetes/manifest/etcd.yaml`.  <br/>
  The ETCD can be part of your cluster (on your worker/master nodes) or on a totally seperate vm/container. 
  
  #### Important files of ETCD
  ETCD have 3 important files that it needs to communicate via the `etcdctl`. These files are mounted into the containers based on the yaml files.
  
  ETCD machine                              ->      ETCD container <br/>
  `/etc/kubernetes/pki/etcd/ca.crt`         ->      `/etc/kubernetes/pki/etcd/ca.crt` <br/>
  `/etc/kubernetes/pki/etcd/server.crt`     ->      `/etc/kubernetes/pki/etcd/server.crt` <br/>
  `/etc/kubernetes/pki/etcd/server.key`     ->      `/etc/kubernetes/pki/etcd/server.key` <br/>

  #### Using etcdctl
  The etcd container comes with a shell and the etcdctl binary installed.
  
  ```
  # Health check
  ETCDCTL_API=3 ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key etcdctl endpoint health
  # List member
  ETCDCTL_API=3 ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key etcdctl member list
  # List member in table format
  ETCDCTL_API=3 ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key etcdctl endpoint health -w table
  # Snapshot. File will be at /var/lib/etcd/snapshot.db of host machine
  ETCDCTL_API=3 ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key etcdctl --endpoints=https://127.0.0.1:2379 snapshot save /var/lib/etcd/snapshot.db
  ```
  
  #### Important files to backup
  * ETCD snapshot
  * kubeadm config map `kubectl get cm kubeadm-config  -n kube-system`
  * certs at `/etc/kubernetes/pki/`
  
  ### Upgrading Cluster
  Snapshot, snapshot, snapshot
  
  #### Update master
  ```
  sudo apt-mark unhold kubeadm
  sudo apt install -i kubeadm=[version]
  sudo apt-mark hold kubeadm
  ```
  
  Check upgrade changes
  ```
  sudo kubeadm upgrade plan [version]
  ```
  Note the sections
  * that needs to be upgraded manually (kubelet, kubectl)
  * for offline deployment, push the updated containers into private registry
  
  Upgrading master node
  ```
  # Drain node
  kubectl drain nodes master --ignore-daemonset
  
  # Upgrade kubeadm
  sudo kubeadm upgrade apply [version]
  # Upgrade kubelet and kubectl
  sudo apt-mark unhold kubelet kubectl
  sudo apt install -y kubelet=[version] kubectl=[version]
  
  # Restart daemon
  sudo systemctl daemon-reload
  sudo systemctl restart kubelet
  
  # Uncordon node
  kubectl uncordon master
  ```
  
  #### Update worker
   ```
  sudo apt-mark unhold kubeadm
  sudo apt install -i kubeadm=[version]
  sudo apt-mark hold kubeadm
  ```
  
  Upgrading Worker node
  ```
  # Drain node
  kubectl drain nodes worker --ignore-daemonset
  
  # Upgrade node
  sudo kubeadm upgrade node
  
  # Upgrade kubelet and kubectl
  sudo apt-mark unhold kubelet kubectl
  sudo apt install -y kubelet=[version] kubectl=[version]
  
  # Restart daemon
  sudo systemctl daemon-reload
  sudo systemctl restart kubelet
  
  # Uncordon node
  kubectl uncordon worker
  ```
  
</details>

## Resource limiting
<details>
  <summary>Limiting resources</summary>
  
  ### Resource limiting on container level
  ```
  ...
        spec:
        containers:
        - image: vish/stress
          imagePullPolicy: Always
          name: stress
          resources:
            limits: 
              memory: "4Gi"
            requests:
              memory: "2500Mi"
  ...
  ```
  

  ### Resource limiting by namespace
  ```
apiVersion: v1
kind: LimitRange
metadata:
  name: low-resource-range
spec:
  limits:
    - default:
        cpu: 1
        memory: 500Mi
      defaultRequest:
        cpu: 0.5
        memory: 100Mi
      type: Container
  ```

## Example Section 3
<details>
  <summary>Click to expand!</summary>
  
  ### Example Note 1
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed convallis semper sapien, et iaculis mauris porttitor ac. Pellentesque felis sem, porta vitae aliquet a, commodo non mauris. Integer elementum risus non eleifend suscipit. Nam ut enim eu felis mattis condimentum. Mauris iaculis enim vitae molestie volutpat. Nullam efficitur aliquam ante, consequat auctor tellus congue pharetra. Morbi diam erat, blandit quis augue at, euismod rutrum elit. Etiam bibendum, nulla et lacinia mattis, turpis velit ultricies metus, nec mattis ante orci nec ex. Nam sit amet sodales enim. Morbi pretium eu augue ac consectetur. 

  ### Example Note 2
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed convallis semper sapien, et iaculis mauris porttitor ac. Pellentesque felis sem, porta vitae aliquet a, commodo non mauris. Integer elementum risus non eleifend suscipit. Nam ut enim eu felis mattis condimentum. Mauris iaculis enim vitae molestie volutpat. Nullam efficitur aliquam ante, consequat auctor tellus congue pharetra. Morbi diam erat, blandit quis augue at, euismod rutrum elit. Etiam bibendum, nulla et lacinia mattis, turpis velit ultricies metus, nec mattis ante orci nec ex. Nam sit amet sodales enim. Morbi pretium eu augue ac consectetur. 
</details>
