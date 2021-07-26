
#Kubernates upgrade from 18.x to 19.x on CentOs-7
  
In order to upgrade to immediate next version, taking the backup of etcd.
  
  sudo cp -r /etc/kubernetes/ backup/
  
  Taking a Snapshot of etcd
  
  ```docker run --rm -v $(pwd)/backup:/backup \
    --network host \
    -v /etc/kubernetes/pki/etcd:/etc/kubernetes/pki/etcd \
    --env ETCDCTL_API=3 \
    k8s.gcr.io/etcd:3.4.3-0 \
    etcdctl --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt \
    --key=/etc/kubernetes/pki/etcd/healthcheck-client.key \
    snapshot save /backup/etcd-snapshot-latest.db ``` 
 
The below command is optional and only relevant if you use a configuration file for kubeadm. Storing this file makes it easy to initialize the master with the exact same configuration as before when restoring it.
  # Backup kubeadm-config
    sudo cp /etc/kubeadm/kubeadm-config.yaml backup/
  
  
<h6> Before you begin the upgrade <h6>
  1.Make sure you read the release notes carefully.
  2.The cluster should use a static control plane and etcd pods or external etcd.
  3.Make sure to back up any important components, such as app-level state stored in a database. kubeadm upgrade does not touch your workloads, only components       internal to Kubernetes, but backups are always a best practice.
  4.Swap must be disabled.
 
  On Master:
  Determine which version to upgrade to 
  # find the version in the list
  # it should look like 1.21.x-0, where x is the latest patch
  
  
  yum list --showduplicates kubeadm --disableexcludes=kubernetes

  
  
