# k8s
## HOW TO CREATE A KUBERNETES CLUSTER

```
#ON_MASTER_NODE

echo -e "209.97.157.212\tmaster" >> /etc/hosts
echo -e "165.227.96.210\tnode1" >> /etc/hosts
echo -e "167.71.249.234\tnode2" >> /etc/hosts
sed -i s/"SELINUX=enforcing"/"SELINUX=permissive"/ /etc/sysconfig/selinux
setenforce 0
cp /etc/fstab /root/ && sed -i /"swap"/d /etc/fstab && swapoff -a
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum update -y kernel && yum install -y docker-ce # reboot if needed
cat <<EOF >  /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
EOF
yum install -y kubelet kubeadm kubectl
systemctl enable docker && systemctl start docker
systemctl enable kubelet && systemctl start kubelet
/etc/systemd/system/multi-user.target.wants/docker.service
At line 14 append:
--exec-opt native.cgroupdriver=systemd
cat <<EOF >>  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
kubeadm init --pod-network-cidr= 10.244.0.0/16
systemctl daemon-reload
systemctl restart docker
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
```
#ON_NODE01

echo "k8-node01" > /etc/hostname 
hostname k8-node01
echo -e "209.97.157.212\tk8-master" >> /etc/hosts
echo -e "165.227.96.210\tk8-node01" >> /etc/hosts
echo -e "167.71.249.234\t k8-node02" >> /etc/hosts
sed -i s/"SELINUX=enforcing"/"SELINUX=permissive"/ /etc/sysconfig/selinux
setenforce 0
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum update -y kernel && yum install -y docker-ce
reboot
cat <<EOF >  /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
EOF
yum install -y kubelet kubeadm kubectl
systemctl enable docker && systemctl start docker
systemctl enable kubelet && systemctl enable kubelet
/etc/systemd/system/multi-user.target.wants/docker.service
At line 14 append:
--exec-opt native.cgroupdriver=systemd
systemctl daemon-reload
systemctl restart docker
cat <<EOF >>  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
kubeadm join 209.97.157.212:6443 --token XXXXXXX.XXXXXXXXXX     --discovery-token-ca-cert-hash sha256:XXXXXXXXXXXXXXXXXXX
```

```
#ON_NODE02

echo "k8-node02" > /etc/hostname 
hostname k8-node02
echo -e "209.97.157.212\tk8-master" >> /etc/hosts
echo -e "165.227.96.210\tk8-node01" >> /etc/hosts
echo -e "167.71.249.234\t k8-node02" >> /etc/hosts
sed -i s/"SELINUX=enforcing"/"SELINUX=permissive"/ /etc/sysconfig/selinux
setenforce 0
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum update -y kernel && yum install -y docker-ce
reboot
cat <<EOF >  /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
EOF
yum install -y kubelet kubeadm kubectl
systemctl enable docker && systemctl start docker
systemctl enable kubelet && systemctl enable kubelet
/etc/systemd/system/multi-user.target.wants/docker.service
At line 14 append:
--exec-opt native.cgroupdriver=systemd
systemctl daemon-reload
systemctl restart docker
cat <<EOF >>  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
kubeadm join 209.97.157.212:6443 --token XXXXXXX.XXXXXXXXXX     --discovery-token-ca-cert-hash sha256:XXXXXXXXXXXXXXXXXXX
```


















