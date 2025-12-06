###
Demo with Kubeadm - Provision VMs with Vagrant

brew install vagrant
#OK

brew install virtualbox
#OK

brew install multipass
#OK


https://github.com/kodekloudhub/certified-kubernetes-administrator-course

git clone https://github.com/kodekloudhub/certified-kubernetes-administrator-course.git
cd certified-kubernetes-administrator-course/kubeadm-clusters/apple-silicon
#OK

#PB
start failed: cannot connect to the multipass socket

#SOL 
sudo launchctl stop com.canonical.multipassd
sudo launchctl start com.canonical.multipassd

#PB
...

#SOL
multipass start

#PB
shell failed: ssh connection failed: 'Failed to connect: No route to host'

#SOL 
No firewall
multipass stop --all
multipass start --all
Reboot Mac book to probably solve the DHCP issue.

#OKOKOK

Continue from ...
https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/kubeadm-clusters/apple-silicon/docs/03-connectivity.md

... bypassing ssh setup from ontrolplane to nodes until now ...
I continue with the Multipass ssh <node>

https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/kubeadm-clusters/generic/04-node-setup.md

#OK

https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/kubeadm-clusters/generic/05-controlplane.md

On the control plane ...

POD_CIDR=10.244.0.0/16
SERVICE_CIDR=10.96.0.0/16
#OK

sudo kubeadm init --pod-network-cidr $POD_CIDR --service-cidr $SERVICE_CIDR --apiserver-advertise-address $PRIMARY_IP
#OK

sudo kubeadm init --pod-network-cidr $POD_CIDR --service-cidr $SERVICE_CIDR --apiserver-advertise-address $PRIMARY_IP
error: flag needs an argument: --apiserver-advertise-address
To see the stack trace of this error execute with --v=5 or higher
So missing PRIMARY_IP
Exporting PRIMARY_IP s ...
And re run
cat <<EOF | sudo tee /etc/default/kubelet
KUBELET_EXTRA_ARGS='--node-ip ${PRIMARY_IP}'
EOF
On all nodes
#OK

On worker nodes ...
sudo kubeadm join 192.168.1.58:6443 --token mcs5m3.wyis5wz1edo5jzjz \
    --discovery-token-ca-cert-hash sha256:1bee514a230653554eee5ad4e024d7325f25e53504106d0c72e3068fb7a13291
#OK

On master
{
    mkdir ~/.kube
    sudo cp /etc/kubernetes/admin.conf ~/.kube/config
    sudo chown $(id -u):$(id -g) ~/.kube/config
    chmod 600 ~/.kube/config
}
And 
kubectl get pods -n kube-system
#OKOKOK

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.1/manifests/tigera-operator.yaml
#OK

curl -LO https://raw.githubusercontent.com/projectcalico/calico/v3.30.1/manifests/custom-resources.yaml
sed -i "s#192.168.0.0/16#$POD_CIDR#" custom-resources.yaml
kubectl apply -f custom-resources.yaml
#OK

watch kubectl get tigerastatus
#OK

kubectl get pods -n kube-system
#OK

#OKOKOK 
#OKOKOK 

#TODO Get the command lines from Demo - Deployment with kubeadm ??? ??? ??? !!! !!! !!!

###
Practice test - Deploy a kubernetes cluster using kubeadm


From ...
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
#OK

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
#Change the version !
#OK

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
#Change the version
#OK

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
#OK

From ...
https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/configure-cgroup-driver/

vi kubeadm-config.yaml
# kubeadm-config.yaml
kind: ClusterConfiguration
apiVersion: kubeadm.k8s.io/v1beta4
kubernetesVersion: v1.21.0
---
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: systemd


POD_CIDR=10.244.0.0/16
SERVICE_CIDR=10.96.0.0/16


kubeadm init --config kubeadm-config.yaml --apiserver-advertise-address 192.168.58.163 --pod-network-cidr $POD_CIDR --service-cidr $SERVICE_CIDR


$[?( @ > 40 )]

$.car.wheels[?(@.location == "rear")].model

cat q13.json | jpath '$[0,3]'

$.*.color

$[*].model

$.*.wheels[*].model

cat q9.json | jpath '$.prizes[*].laureates[*]'
cat q9.json | jpath '$.prizes[*].laureates[?(@.id==913)]'

$[0:3]
$[0,3]
$[0:8:2] #by step
$[-1] #the last
$[-1:0] #the last
$[-3:] #the 3 last elements

 
###
Json Path for kubernetes

kubectl get pods -o=jsonpath'{ .items[0].spec.containers[0].image }'

- No need for $, added by kubectl
- Command must be included in '{ }'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'

{range .items[*]}
  {.metadata.name}{"\t"}
  {.status.capacity.cpu}{"\n"}
{end}

kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'

kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU:.status.capacity.cpu

kubectl get nodes --sort-by=.metadata.name

kubectl get pv --sort-by=.spec.capacity.storage > /opt/outputs/storage-capacity-sorted.txt

kubectl get pv --sort-by=.spec.capacity.storage -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage 

kubectl config view --kubeconfig /root/my-kube-config -o=jsonpath='{.contexts[0].name}' > /opt/outputs/aws-context-name



########


Light exam

sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.33.0-1.1' && \
sudo apt-mark hold kubeadm

sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.33.0-1.1' kubectl='1.33.0-1.1' && \
sudo apt-mark hold kubelet kubectl

systemctl daemon-reload
systemctl restart kubelet


#######

Install a Debian package

dpkg -i ...

systemctl start cri-docker
systemctl enable cri-docker


############

To expose a pod as a service !!!

kubectl expose pod messaging --port xxxx --name messaging-service 

OR

kubectl expose deployment hr-webapp --type=nodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml
#Then vi and change the nodePort

$[?( @ > 40 )]

$.car.wheels[?(@.location == "rear")].model

cat q13.json | jpath '$[0,3]'

$.*.color

$[*].model

$.*.wheels[*].model

cat q9.json | jpath '$.prizes[*].laureates[*]'
cat q9.json | jpath '$.prizes[*].laureates[?(@.id==913)]'

$[0:3]
$[0,3]
$[0:8:2] #by step
$[-1] #the last
$[-1:0] #the last
$[-3:] #the 3 last elements

 
###
Json Path for kubernetes

kubectl get pods -o=jsonpath'{ .items[0].spec.containers[0].image }'

- No need for $, added by kubectl
- Command must be included in '{ }'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'

{range .items[*]}
  {.metadata.name}{"\t"}
  {.status.capacity.cpu}{"\n"}
{end}

kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'

kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU:.status.capacity.cpu

kubectl get nodes --sort-by=.metadata.name

kubectl get pv --sort-by=.spec.capacity.storage > /opt/outputs/storage-capacity-sorted.txt

kubectl get pv --sort-by=.spec.capacity.storage -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage 

kubectl config view --kubeconfig /root/my-kube-config -o=jsonpath='{.contexts[0].name}' > /opt/outputs/aws-context-name



########


Light exam

sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.33.0-1.1' && \
sudo apt-mark hold kubeadm

sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.33.0-1.1' kubectl='1.33.0-1.1' && \
sudo apt-mark hold kubelet kubectl

systemctl daemon-reload
systemctl restart kubelet


#######

Install a Debian package

dpkg -i ...

systemctl start cri-docker
systemctl enable cri-docker


############

To expose a pod as a service !!!

kubectl expose pod messaging --port xxxx --name messaging-service 

OR

kubectl expose deployment hr-webapp --type=nodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml
#Then vi and change the nodePort

$[?( @ > 40 )]

$.car.wheels[?(@.location == "rear")].model

cat q13.json | jpath '$[0,3]'

$.*.color

$[*].model

$.*.wheels[*].model

cat q9.json | jpath '$.prizes[*].laureates[*]'
cat q9.json | jpath '$.prizes[*].laureates[?(@.id==913)]'

$[0:3]
$[0,3]
$[0:8:2] #by step
$[-1] #the last
$[-1:0] #the last
$[-3:] #the 3 last elements

 
###
Json Path for kubernetes

kubectl get pods -o=jsonpath'{ .items[0].spec.containers[0].image }'

- No need for $, added by kubectl
- Command must be included in '{ }'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'

{range .items[*]}
  {.metadata.name}{"\t"}
  {.status.capacity.cpu}{"\n"}
{end}

kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'

kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU:.status.capacity.cpu

kubectl get nodes --sort-by=.metadata.name

kubectl get pv --sort-by=.spec.capacity.storage > /opt/outputs/storage-capacity-sorted.txt

kubectl get pv --sort-by=.spec.capacity.storage -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage 

kubectl config view --kubeconfig /root/my-kube-config -o=jsonpath='{.contexts[0].name}' > /opt/outputs/aws-context-name



########


Light exam

sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.33.0-1.1' && \
sudo apt-mark hold kubeadm

sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.33.0-1.1' kubectl='1.33.0-1.1' && \
sudo apt-mark hold kubelet kubectl

systemctl daemon-reload
systemctl restart kubelet


#######

Install a Debian package

dpkg -i ...

systemctl start cri-docker
systemctl enable cri-docker


############

To expose a pod as a service !!!

kubectl expose pod messaging --port xxxx --name messaging-service 

OR

kubectl expose deployment hr-webapp --type=nodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml
#Then vi and change the nodePort

$[?( @ > 40 )]

$.car.wheels[?(@.location == "rear")].model

cat q13.json | jpath '$[0,3]'

$.*.color

$[*].model

$.*.wheels[*].model

cat q9.json | jpath '$.prizes[*].laureates[*]'
cat q9.json | jpath '$.prizes[*].laureates[?(@.id==913)]'

$[0:3]
$[0,3]
$[0:8:2] #by step
$[-1] #the last
$[-1:0] #the last
$[-3:] #the 3 last elements

 
###
Json Path for kubernetes

kubectl get pods -o=jsonpath'{ .items[0].spec.containers[0].image }'

- No need for $, added by kubectl
- Command must be included in '{ }'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'

{range .items[*]}
  {.metadata.name}{"\t"}
  {.status.capacity.cpu}{"\n"}
{end}

kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'

kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU:.status.capacity.cpu

kubectl get nodes --sort-by=.metadata.name

kubectl get pv --sort-by=.spec.capacity.storage > /opt/outputs/storage-capacity-sorted.txt

kubectl get pv --sort-by=.spec.capacity.storage -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage 

kubectl config view --kubeconfig /root/my-kube-config -o=jsonpath='{.contexts[0].name}' > /opt/outputs/aws-context-name



########


Light exam

sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.33.0-1.1' && \
sudo apt-mark hold kubeadm

sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.33.0-1.1' kubectl='1.33.0-1.1' && \
sudo apt-mark hold kubelet kubectl

systemctl daemon-reload
systemctl restart kubelet


#######

Install a Debian package

dpkg -i ...

systemctl start cri-docker
systemctl enable cri-docker


############

To expose a pod as a service !!!

kubectl expose pod messaging --port xxxx --name messaging-service 

OR

kubectl expose deployment hr-webapp --type=nodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml
#Then vi and change the nodePort
