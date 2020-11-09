To Create Wordpress application with MYSQL database

1: Configure Kubernetes Repository

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

2.sudo yum install -y kubelet kubeadm kubectl
3.systemctl enable kubelet
4. systemctl start kubelet
5. sudo hostnamectl set-hostname master-node
6. sudo hostnamectl set-hostname worker-node1
7. sudo vi /etc/hosts (Add server ips in hosts files)
8. 
Introduction
Small virtual environments, called containers, have become indispensable for developing and managing applications.

Working on applications within an isolated container does not affect the host operating system. Containers are more efficient than virtual machines as they do not need their operating system.

Kubernetes is an open-source platform that helps you deploy, scale, and manage resources across multiple containers.

Follow this tutroial and learn how to install Kubernetes on a CentOS 7 system.

Tutorial on how to install and deploy Kubernetes on CentOS 7.
Prerequisites
Multiple Linux servers running CentOS 7 (1 Master Node, Multiple Worker Nodes)
A user account on every system with sudo or root privileges
The yum package manager, included by default
Command-line/terminal window
Steps for Installing Kubernetes on CentOS 7
To use Kubernetes, you need to install a containerization engine. Currently, the most popular container solution is Docker. Docker needs to be installed on CentOS, both on the Master Node and the Worker Nodes.

Step 1: Configure Kubernetes Repository
Kubernetes packages are not available from official CentOS 7 repositories. This step needs to be performed on the Master Node, and each Worker Node you plan on utilizing for your container setup. Enter the following command to retrieve the Kubernetes repositories.

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
Note: If using the sudo command, append it not only to the cat command but to the restricted file as well.

Step 2: Install kubelet, kubeadm, and kubectl
These 3 basic packages are required to be able to use Kubernetes. Install the following package(s) on each node:

sudo yum install -y kubelet kubeadm kubectl
systemctl enable kubelet
systemctl start kubelet
You have now successfully installed Kubernetes, including its tools and basic packages.

System confirms that you have installed kubeadm, kubectl and kubelet
Before deploying a cluster, make sure to set hostnames, configure the firewall, and kernel settings.

Step 3: Set Hostname on Nodes
To give a unique hostname to each of your nodes, use this command:

sudo hostnamectl set-hostname master-node
or

sudo hostnamectl set-hostname worker-node1
In this example, the master node is now named master-node, while a worker node is named worker-node1.

Make a host entry or DNS record to resolve the hostname for all nodes:

sudo vi /etc/hosts

On the Master Node enter:

sudo firewall-cmd --permanent --add-port=6443/tcp
sudo firewall-cmd --permanent --add-port=2379-2380/tcp
sudo firewall-cmd --permanent --add-port=10250/tcp
sudo firewall-cmd --permanent --add-port=10251/tcp
sudo firewall-cmd --permanent --add-port=10252/tcp
sudo firewall-cmd --permanent --add-port=10255/tcp
sudo firewall-cmd --reload


Introduction
Small virtual environments, called containers, have become indispensable for developing and managing applications.

Working on applications within an isolated container does not affect the host operating system. Containers are more efficient than virtual machines as they do not need their operating system.

Kubernetes is an open-source platform that helps you deploy, scale, and manage resources across multiple containers.

Follow this tutroial and learn how to install Kubernetes on a CentOS 7 system.

Tutorial on how to install and deploy Kubernetes on CentOS 7.
Prerequisites
Multiple Linux servers running CentOS 7 (1 Master Node, Multiple Worker Nodes)
A user account on every system with sudo or root privileges
The yum package manager, included by default
Command-line/terminal window
Steps for Installing Kubernetes on CentOS 7
To use Kubernetes, you need to install a containerization engine. Currently, the most popular container solution is Docker. Docker needs to be installed on CentOS, both on the Master Node and the Worker Nodes.

Step 1: Configure Kubernetes Repository
Kubernetes packages are not available from official CentOS 7 repositories. This step needs to be performed on the Master Node, and each Worker Node you plan on utilizing for your container setup. Enter the following command to retrieve the Kubernetes repositories.

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
Note: If using the sudo command, append it not only to the cat command but to the restricted file as well.

Step 2: Install kubelet, kubeadm, and kubectl
These 3 basic packages are required to be able to use Kubernetes. Install the following package(s) on each node:

sudo yum install -y kubelet kubeadm kubectl
systemctl enable kubelet
systemctl start kubelet
You have now successfully installed Kubernetes, including its tools and basic packages.

System confirms that you have installed kubeadm, kubectl and kubelet
Before deploying a cluster, make sure to set hostnames, configure the firewall, and kernel settings.

Step 3: Set Hostname on Nodes
To give a unique hostname to each of your nodes, use this command:

sudo hostnamectl set-hostname master-node
or

sudo hostnamectl set-hostname worker-node1
In this example, the master node is now named master-node, while a worker node is named worker-node1.

Make a host entry or DNS record to resolve the hostname for all nodes:

sudo vi /etc/hosts
With the entry:

192.168.1.10 master.phoenixnap.com master-node
192.168.1.20 node1. phoenixnap.com node1 worker-node
Step 4: Configure Firewall
The nodes, containers, and pods need to be able to communicate across the cluster to perform their functions. Firewalld is enabled in CentOS by default on the front-end. Add the following ports by entering the listed commands.

On the Master Node enter:

sudo firewall-cmd --permanent --add-port=6443/tcp
sudo firewall-cmd --permanent --add-port=2379-2380/tcp
sudo firewall-cmd --permanent --add-port=10250/tcp
sudo firewall-cmd --permanent --add-port=10251/tcp
sudo firewall-cmd --permanent --add-port=10252/tcp
sudo firewall-cmd --permanent --add-port=10255/tcp
sudo firewall-cmd --reload
Each time a port is added the system confirms with a ‘success’ message.

Adding ports to firewalld exceptions
Enter the following commands on each worker node:

sudo firewall-cmd --permanent --add-port=10251/tcp
sudo firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --reload

Use following commands to disable SELinux:

sudo setenforce 0
sudo sed -i ‘s/^SELINUX=enforcing$/SELINUX=permissive/’ /etc/selinux/config

sudo sed -i '/swap/d' /etc/fstab
sudo swapoff -a

Create Cluster with kubeadm

sudo kubeadm init --pod-network-cidr=10.244.0.0/16

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

Set Up Pod Network

sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

sudo kubectl get nodes

sudo kubectl get pods --all-namespaces

Worker Node join cluster

kubeadm join --discovery-token cfgrty.1234567890jyrfgd --discovery-token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443


Configure the wordpress-deployment.yaml in linux server.

Configure the mysql-deployment.yaml in linux server

kubectl apply -k ./

kubectl get secrets

kubectl get pvc

kubectl get pods

kubectl get services wordpress

minikube service wordpress --url
