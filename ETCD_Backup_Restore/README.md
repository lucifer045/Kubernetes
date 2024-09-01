#Process to Backup ETCD#

(1) Check for endpoint on which ETCD is listening

     cat /etc/kubernetes/manifests/etcd.yaml | grep listen

(2) Check for --cert, --cacert, --key

     cat /etc/kubernetes/manifests/etcd.yaml | grep file

(3) Set the ETCDCTL_API=3

     export ETCDCTL_API=3

(4) Create the snapshot 

     etcdctl --endpoints=https://127.0.0.1:2379 \
     --cacert=/etc/kubernetes/pki/etcd/ca.crt \
     --cert=/etc/kubernetes/pki/etcd/server.crt \
     --key=/etc/kubernetes/pki/etcd/server.key \
     snapshot save /opt/etcdboot.db

#Process to Restore ETCD#

(5) Restore etcd from the snapshot

     etcdctl --data-dir="/var/lib/etcdboot" \
     --endpoints=https://127.0.0.1:2379 \
     --cacert=/etc/kubernetes/pki/etcd/ca.crt \
     --cert=/etc/kubernetes/pki/etcd/server.crt \
     --key=/etc/kubernetes/pki/etcd/server.key \
     snapshot restore /opt/etcdboot.db

(6) Now make changes in etcd.yaml file 

     vi /etc/kubernetes/manifests/etcd.yaml

Change the --data-dir value and mountPath and Path 

(7) Verify 

     kubectl get pods

