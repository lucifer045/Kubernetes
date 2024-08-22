## UPGRADING kubeadm CLUSTER

# Point to be noted

=> Upgrade Primary Control Plane node
=> Upgrade additional control plane node
=> Upgrade Worker nodes

# Process of Upgrade:

(1) Check the Operating system you are working on:

         cat /etc/*release*  #In this case Ubuntu is OS

(2) Check the current kubeadm version

         kubeadm version

(3) Upgrade kubeadm

Use any text editor you prefer to open the file that defines the Kubernetes apt repository and upgrade the version in the URL to the next available minor release i.e. v1.30

         vim /etc/apt/sources.list.d/kubernetes.list

After making changes install the latest version using the below command

          sudo apt-mark unhold kubeadm && sudo apt-get update && sudo apt-get install -y kubeadm='1.30.0-1.1' && sudo apt-mark hold kubeadm

Now upgrade the Kubernetes cluster

          sudo kubeadm upgrade plan v1.30.0

          sudo kubeadm upgrade apply v1.30.0

(4) Drain the controlplane node

          kubectl drain controlplane --ignore-daemonsets

(5) Upgrade the kubelet and kubectl

          sudo apt-mark unhold kubelet kubectl && sudo apt-get update && sudo apt-get install -y kubelet='1.30.0-1.1' kubectl='1.30.0-1.1' && sudo apt-mark hold kubelet kubectl

(6) Restart the kubelet

          sudo systemctl daemon-reload

          sudo ststemctl restart kubelet

(7) Uncordon the node

          kubelet uncordon controlplane


#### In the similar way upgrade the other nodes too.
