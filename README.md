# CKA

Book the exam.

Book the exam
https://trainingportal.linuxfoundation.org/learn/dashboard/
-> Resume
There is availability for friday morning for instance ... in two weeks ...

Take the killer.sh (2 times)
https://trainingportal.linuxfoundation.org/learn/course/certified-kubernetes-administrator-cka/exam/exam


Use code 30KK to get 30% off.

TODO
Configure KodeKloud on discord.
https://discord.gg/VAfhT6ZR9E


## LINKS

https://notes.kodekloud.com/docs/CKA-Certification-Course-Certified-Kubernetes-Administrator/Introduction/Course-Introduction

Authorized documentation for the exam

https://kubernetes.io/docs

https://kubernetes.io/blog/

https://helm.sh/docs

https://gateway-api.sigs.k8s.io/


## MY LINKS

The bookmark is no more available.

https://killercoda.com/ !!! !!! !!!

https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/


https://raghu.sh/new-cka-exam-tips-tricks-ft-gateway-api-cni-cri-csi-helm-kustomize-ab5bb621b914
# the new CKA !!!
# lot of good informations
- Use VSCode
- Use of -o jsonpath
- Migrate from ingress to gateway
- Installing helm charts w/o crds
- Use of helm template 
- Use of openssl to validate Certs
- Types of CNI and their limitations
- crictl to debug pods on nodes
- Call to k8s API using a secret mounted in a pod

https://devopscube.com/
https://devopscube.com/cka-exam-study-guide/


## QUESTIONS

Only one monitor is accepted ?
- Replicate the monitors.
- Clap the Mac book and use external keyboard, mouse, camera, and micro.
- https://github.com/waydabber/BetterDisplay
- ...

Where can I find the documentation about kustomize ?
Can I use the sig kubernetes GitHub kustomize documentation ?k
How could I get the same as kubectl explain kustomization ?


## TODO

Take the official simulation: 2 mock exams
Killer.sh

ID card needed for the exam.

## Register for the exam

Should use the permis de conduire as id document

Must run the exam on Chrome because safari is failing the test and arc has a problem with the microphone but is valid

Peu de dispo avant deux semaines ...
Idéalement je passe l'examen un mardi de 10:00 à 12:00

## CKA Content.

### Controller manager:
- running all the controllers
- responsible for managing the state of the cluster
- running in the kube-system namespace
- kube-controller-manager
- controllers are responsible for managing the state of the cluster

### scheduler:
- responsible for scheduling pods on nodes
- running in the kube-system namespace
- kube-scheduler
- scheduling decisions based on resource availability, affinity, taints, tolerations, etc.
- kube-scheduler is the default scheduler
- scheduler can be customized with custom schedulers

### kubelet:
- responsible for managing the state of the node
- running on each node
- kubelet is responsible for managing the pods on the node
- kubelet is responsible for managing the containers on the node
- kubelet is responsible for managing the node itself
- kubelet is responsible for managing the node's resources

### kube proxy:
- responsible for managing the network connectivity of the pods
- running on each node
- kube-proxy is responsible for managing the network connectivity of the pods and the services
- using ip tables or ipvs to manage the network connectivity

### Pods:
- kind
- apiVersion
- metadata
- spec

```
kubectl run redis --image redis123 --dry-run=client -o yaml > redis.yaml
#OK
```

replicaset:
- the replication controller is responsible for managing the replicas of a pod
- the selector is mandatory in the replicaset not in the replication controller
- spec:
    template:
    replicas:
    selector:

Tips:
```
kubectl run nginx --image=nginx --dry-run=client -o yaml
#OK

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
#OK

kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml
#OK

kubectl create deployment --image=httpd:2.4-alpine httpd-frontend --replicas=3 --dry-run=client -o yaml
#OK
```

## Service
- nodeport (from the service perspective)
  - nodeprort #on each node in the range 30000 - 32767
  - port #on the clusterIP
  - targetPort #of the container
- clusterIP
  - port #of the service
  - targetPort #of the container
- loadBalancer
  - nodeprort #on each node in the range 30000 - 32767
  - port #on the LB for external clients
  - targetPort #of the container

##

## Exam tips
```
kubectl run --image=nginx nginx
#OK

kubectl create deploy --image=nginx nginx
#OK

kubectl expose deploy nginx --port=80
#OK

kubectl edit deploy nginx
#OK

kubectl scale deploy nginx --replicas=5
#OK

kubectl set image deploy nginx nginx=nginx:1.21
#OK

#create a service !!! !!! !!!
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
#OK

kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
#OK

kubectl expose ... -> need edit to change nodePort

kubectl expose --help
#recommended !!!


kubectl expose deployment hr-webapp --type=nodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml
#Then vi and change the nodePort because this is not an option
```

## Imperative

Resource            Trick to Generate
- DaemonSet           Create Deployment YAML -> Change Kind -> Delete replicas
- StatefulSet         Create Deployment YAML -> Change Kind -> Add serviceName
- Ingress             kubectl create ingress ... (It exists!)
- CronJob             kubectl create cronjob ...
- Service (NodePort)  kubectl expose deploy ... --type=NodePort --dry-run=client -o yaml
- PVC / NetPol        No generator. Use kubectl explain or copy existing.


Imperative commands
```
kubectl run httpd --image=httpd:alpine --port=80 --expose
service/httpd created
pod/httpd created
#OK
```

### Labels and selectors
```
kubectl get pods --selector app=app1
```
##

### Taints - Toleration
- NoSchedule
- PreferNoSchedule
- NoExecute #only new pods will be placed on the node and running pods w/o the tolerance are ejected
* It is used to evict a pod to run on a specific node.
* To let a pod run on a specific node, we will use affinity.
* Imaginez un taint (une souillure 🤢) comme un panneau "🚫 Interdit aux non-autorisés" sur un nœud de votre cluster Kubernetes.
* Ce panneau est là pour empêcher les pods ordinaires de s'exécuter sur ce nœud. 

The toleration could match the key with the operator exists or compare the key to a value with an operator.

```yaml
tolerations:
- key: "node.kubernetes.io/unreachable"
  operator: "Exists"
  effect: "NoExecute"
  tolerationSeconds: 6000
```

```yaml
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoExecute"
  tolerationSeconds: 3600
```

## 
## Tmux
```
tmux ls
tmux attach -t
Ctrl-B % vertical split
Ctrl-B " horizontal split
Ctrl-B o move to the next pane
Ctrl-B x exit
Ctrl-B d detach
#OK
```

##

### More exam tips
```
kubectl node explain --recursive 
#OKOKOK 

kubectl explain deploy --recursive | grep replicas -C 5
#OK

Node selector (has limitations)
Kubectl label nodes node-1 size=Large
On the pod ...
spec:
  nodeSelector:
    size: Large

Node affinity
spec:
  nodeAffinity:
    required...
      nodeSelectorTerms:
      - matchExpressions:
        - key: size
          operator: In
          values:
          - Large
DuringScheduling => new pods
DuringExecution => existing pods

kubectl create deployment my-dep --image=nginx --replicas=3
#OK 
```

##

### Resources and limits

- Best choice is request but no limits
- LimitRange: to create default limit and requests on the ns
- Resource quotas: hard limits for requests and limits in the ns

##

### Daemonsets
- Uses nodeAffinity and default scheduler
- It was using nodeName previously and it was scheduled by the daemonsetController

##

### Static pods
- A static pod is a pod that is managed directly by the kubelet on a specific node
- In /etc/kubernetes/manifests #checked by kubelet w/o kube scheduler
- Only for create pods ...
- Kubelet work at the pod level
- Staticpodpath or kubeconfig
- Docker ps or crictl ps
- Static pods for the control plane components they are ignored by kubescheduler
  - Chicken and egg problem: This allows the essential control plane services to start up before the API server is fully operational, solving the bootstrapping challenge.
```
kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
#OK
```

##

### Priority classes
- Not used
- From 1 000 000 000 to -2 000 000 000
- Kube-system from 2 000 000 000
- globalDefault: true
- preemptionPolicy: PreemptLowerPriority #by default

```
kubectl get pods -o custom-columns="NAME:.metadata.name,PRIORITY:.spec.priorityClassName"
```

- Note remove priority when adding priorityClassName !!!

- There are two high priority classes by default: system-cluster-critical and system-node-critical.
- By default a pod get a priority of 0.

## 

### Multiple scheduler

KubeSchedulerConfiguration
```
Scheduler as a pod
--config=/etc/kubernetes/xxx.yaml
```
```yaml
leaderElection:
  leaderElect: true
```

In the pod ...
```yaml
  schedulerName: xxx
```
```
kubectl get events -o wide
kubectl logs xxx ...
```

Search for multiple kube scheduler

##

## Configuring Schduler Profile

### Plugins
1. Scheduling queue -> ProritySort 
2. Filtering -> NodeResourcesFit and NodeName and NodeUnschedulable
3. Scoring -> NodeResourceFit and ImageLocality 
4. Binding -> DefaultBinder

ExtensionPoints 
...

Multiple profiles in one KubeschedulerConfiguration

##

### Admission controllers
- AlwaysPullImages
- DefaultStorageClass
- EventRateLimit
- NamespaceLifecycle
- NamespaceExists
- ...

Which admission control plugins are enabled ?
```
kube-apiserver -h | grep enable-admission-plugins
kubectl exec kube-api-server-controlplane -n kube-system -- kube-apiserver -h | grep enable-admission-plugins
grep enable-admission-plugins /etc/kubernetes/manifests/kube-apiserver.yaml
ps -ef | grep kube-apiserver | grep admission-plugins !!!
#OK
```

##

Validating and mutating admission controllers

- Mutating run before validating
- For custom Mutating/validatingAdmissionWebhook

- Admission Webhook Server must run
- And configure web hook.

##

### Monitor cluster components

- Metrics server is in memory server so no disk 
- Kubelet use cAdvisor
- Metric server is external and should be installed
- kubctl top node/pod

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

##

### Types of metrics

* Metrics
- for memory and cpu

* Custom metrics
- within the cluster
- the application itself
- for example http requests

* External metrics
- from outside the cluster or the application
- the number of messages in a pub/sub queue

Monitor memory and cpu consumed by containers.
```
kubectl top pods <pod-name> --containers --sort-by=memory
```

##

### Application logs

```
kubectl logs -f xxx
kubectl logs -f pod-name container-name 
```

##

### Deployment updates and rollback

```
kubectl rollout status deploy/xxxx
kubectl rollout history deploy/xxxx
```

2 deploy strategy
- Recreate
- Rollingupdate #default

Upgrade creates a new replicates

Kubectl rollout undo deploy/xxxx

kubectl set image deployments/frontend simple-webapp=kodekloud/webapp-color:v2

##

### Application commands

```
FROM Ubuntu

ENTRYPOINT ["sleep"]

CMD ["5"]
```

##

### Commands and arguments

```yaml
spec:
  Containers:
    - name: ubuntu-sleeper
      Image: ubuntu-sleeper
      Args: ["10"] #-> CMD
      Command: ["sleep2.0"] #-> ENTRYPOINT
```
!!! !!! !!!

##

### Configure Env Var

```
env:
  - name: APP_COLOR
    Value: pink

env:
  - name: APP_COLOR
    ValueFrom:
      configMapKeyRef:
        Name: XXX
        Key: xxxx

env:
  - name: APP_COLOR
    ValueFrom:
      secretKeyRef: xxxx
        Name: XXX
        Key: xxxx
```

##

### ConfigMaps

- From doc Concepts / Configuration 
- From doc Tasks / Configure pods and containers

So 3 possibilites
- Single value
- Config map
- Volume

In pod with CM ...
```yaml
spec:
  container:
    - name:
      envFrom:
        - configMapRef:
            name: xxxx
```
In pod with volumes ...
```yaml
volumes:
  - name:
      configMap:
        name: xxx
```
##

### Secrets

- From doc Concepts / Configuration 
- From doc Tasks / Configure pods and containers

3 possibilities
- Single value
- Single env
- Volume

```yaml
envFrom: 
  - secretRef:
    name: app-config

env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: xxx
        key: XXX

volumes:
- name: app-secret-volume
    secret:
      secretName: xxxx
```
##

### Encrypting Secret Data At Rest

https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

```
apt-get install etcd-client

ETCDCTL_API=3 etcdctl \
   --cacert=/etc/kubernetes/pki/etcd/ca.crt   \
   --cert=/etc/kubernetes/pki/etcd/server.crt \
   --key=/etc/kubernetes/pki/etcd/server.key  \
   get /registry/secrets/default/secret1 | hexdump -C


ps -aux | grep kube-api | grep "encryption-provider-config"

vi /etc/kubernetes/manifests/kube-api-server.yaml
```
Create the EncryptionConfiguration file with aescbc provider ...
```
head -c 32 /dev/urandom | base64

Adding to kubeapi-server ...

Restart kubeapi-server
crictl pods 
#OK
```
##

### Multi-container pods

- Lifecycle
- network
- Storage

##

### Multi-container design pattern

- Col-located containers # share l n s # no order defined
- Regular init-container # setup
- Sidecar container # logger 

### Co-located containers

### Regular init-container

```yaml
initContainers:
  - name: 
     image:
     command:
```

### Sidecar container #is an init container but stay alive

```yaml
containers:

initContainers:
  - name:
     image:
     command:
     restartPolicy: Always #-> sidecar
```
https://learn.kodekloud.com/user/courses/cka-certification-course-certified-kubernetes-administrator/module/2ddcf79b-abb0-4aeb-ad0c-3d54c7b4fc64/lesson/27822812-d758-428d-91db-942db6800ab1 !!!

## Intro to autoscaling

- Cluster auto scaler (node)
- HPA
- VPA

### HPA

kubectl scale can be used to scale deployments and statefullsets

Missing resource metrics -> because of missing resource requirements at the pod ??? 

### VPA 

FEATURE_GATES=InPlacePodVerticalScaling=true

resize_olicy:
  - resourceName: cpu
     restartPolicy: NotRequired
  - resourceName: memory
     restartPolicy: RestartContainer

Only for CPU and memory

VPA must be deployed

- VPA admission controller #check metric-server
- VPA updater #kills the pod
- VPA recommander #modify the resources

Modes
- Off
- Initial #on pod creation
- Recreate
- Auto #like recreate in wait for in place update

VPA best for stateful workload, CPU/memory heavy apps (db, MLs)
HPA is best for web apps, MS, stateless

VPA restarts pods until in place update
```
kubectl apply -f /root/vpa-crds.yml
kubectl apply -f /root/vpa-rbac.yml

git clone https://github.com/kubernetes/autoscaler.git
cd autoscaler/vertical-pod-autoscaler
```
## OS Upgrades

```
kubectl drain node-1 --ignore-daemonsets
kubectl cordon node-1
kubectl uncordon node-1
```

### Kubernetes releases

Major.minor.patch

##
Cluster upgrade

Kube-apiserver
X

Controller manager		Kube-scheduler
X-1						        X-1

Kubectl 
X+1 > X-1

Kubelet					Kube-proxy
X-2						  X-2

The last three versions are supported so X-2

We need to upgrade one minor at a time.

First upgrade the master then the nodes.

### Kubernetes upgrades.

kubeadm upgrade plan

kubeadm does not upgrade kubelet

You must upgrade kubeadm itself before to upgrade the cluster.

1. Master
```
apt-get upgrade -y kubeadm=1.x.x-00
apt-get upgrade -y kubelet=1.12.0-00 #if running on master
kubeadm upgrade apply v1.12.0
kubectl get nodes #provide the kubelet versions not the master
systemctl restart kubelet
```

2. Workers
```
kubectl drain node01
apt-get upgrade -y kubeadm=1.12.0-00
apt-get upgrade -y kubelet=1.12.0-00
kubeadm upgrade node config --kubelet-version v1.12.0
systemctl restart kubelet
kubectl uncordon node01
````

https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

```
vi /etc/apt/sources.list.d/kubernetes.list

deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /
#OK

apt update

apt-cache madison kubeadm
#OK

apt-get install kubeadm=1.33.0-1.1
#OK

kubeadm upgrade plan v1.33.0
#OK

kubeadm upgrade apply v1.33.0
#OK

apt-get install kubelet=1.33.0-1.1
#OK

systemctl daemon-reload
#OK 

systemctl restart kubelet
#OK 
```
For node same but ...
```
kubeadm upgrade node
#OK
```
###
Backup and restore

Etcd
PV

Velero or Ark for backups

Data-dir from etc
```
etcdctl snapshot save
etcdctl snapshot restore

Kube api-server stop
etcdctl restore
-> new data dir created

systemctl daemon-reoad
service etcd restart
```

Check certificates expiration with kubeadm
```
kubeadm certs renew apiserver
kubeadm certs check-expiration
```

Join the node

```
kubectl get node # only the controlplane node available

kubeadm token create --print-join-command

ssh node01
    # execute command printed out by command above
    kubeadm join 172.30.1.2:6443 --token ...
    exit

kubectl get node # should show the node
```

Upgrade components of the cluster to next version.

```
# see possible versions
kubeadm upgrade plan

# show available versions
apt-cache show kubeadm # -> good to see available versions

# can be different for you
apt-get install kubeadm=1.34.1-1.1

# could be a different version for you, it can also take a bit to finish!
kubeadm upgrade apply v1.34.1

# can be a different version for you
apt-get install kubectl=1.34.1-1.1 kubelet=1.34.1-1.1

service kubelet restart
```


### etcd

Working with etcdctl and etcdutl

TODO

### Kubernetes security primitives

### Authentication

- Static password file
- Static token file
- Certificates
- Identity services (LDAP)

## TLS introduction

### TLS Basics

- We use asymmetric key to exchange a symmetric key.
- The symmetric key is used to encrypt the traffic.
- On the server there is a public asymmetric and private symmetric key.

- The server public key is associated with a certificate ensuring the validity of the server.
- The certificate is issued by a certificate authority, and not self signed.
- The browser will trust the certificate.
- The CAs use public private keys to sign their certificates.
- The public keys of the CAs are builtin the browser.
- The public keys of your Public Key Infrastructure can also been stored into your browser.

- You can get a certificate by sending a CSR to a CA.

A certificate is a public (assymmetric) key. In facts it includes:
- a public key
- the identity that owns the key (domain name, organization)
- information about the CA that issued the certificate
- a digital signature from the CA
The purpose of the certificate is to provide a way for you to trust that a public key truly belongs to the entity it claims to represent. 

The CA.crt root should be distributed to all.


### TLS kubernetes

There are client (certificate and private key) and server (certificate and private key) certificates and there is a certificate (certificate and private key) authority.


##

###Certificate details.

Openssl tool.

Memotechnique: GENRSA(key) - REQ(csr) - X509(crt)

1. CA.
```
openssl genrsa -out ca.key 2048
-> ca.key
```
```
openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
-> ca.csr
````
```
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
-> ca.crt
````

2. Client certificates : Admin user
```
openssl genrsa -out admin.key 2048
-> admin.key
```
```
openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
-> admin.csr
```
```
openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
-> admin.crt
```

3. Server certificates
```
openssl genrsa -out apiserver.key 2048
-> apiserver.key
```
```
openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr -config openssl.cnf

openssl.cnf
[alt_names]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster.local
-> apiserver.csr
```
```
openssl x509 -req -in apiserver.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out apiserver.crt -extensions v3_req -extfile openssl.cnf -day 1000
-> apiserver.crt
```
##

### View certificate details
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml

openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```
https://github.com/mmumshad/kubernetes-the-hard-way/tree/master/tools

Certificates requirements
https://kubernetes.io/docs/setup/best-practices/certificates/

```
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep crt
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
#OK
```

### Use of openssl

The typical openssl usage is genrsa - req - x509
```
openssl genrsa -out ca.key 2048
-> ca.key

openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
-> ca.csr

openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
-> ca.crt
````

Check certificate validity
```
openssl x509 -in certificate.crt -noout -text

openssl s_client -connect google.com:443 2>/dev/null | openssl x509 -noout -text
```

##

### crictl

```
crictl ps

crictl logs 1234567
#OK
```
##
Certificates API

https://kubernetes.io/docs/tasks/tls/certificate-issue-client-csr/
```
openssl req -in sample.csr -noout -text

cat akshay.csr | base64 -w 0

vi Akshay.csr
```
```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
  - system:authenticated
  request: <Paste the base64 encoded value of the CSR file>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```

##

### Kubeconfig
```
kubectl config view
kubectl config use-context prod-user@production

kubectl config --kubeconfig=/root/my-kube-config use-context research

export KUBECONFIG=/root/my-kube-config
```
##

API Groups
```
Kubectl proxy #is not kube proxy
curl http://localhost:8001 -k
```
##

### Authorization

- Node #kubelet with certificate
- ABAC
- RBAC
- Webhook

Attribute based authorization
Policy file to edit. Difficult to manage.

Role based access control.
Role <- groups, users

Webhook
Used to outsource authorization externally.
Open policy agent.

Authorization-mode=Nodde,RBAC,Webhook #when denied pass to the next module

### RBAC

```
kubectl auth can-i create deployments
kubectl auth can-i delete pods
kubectl auth can-i create deployments --as dev-user
kubectl auth can-i delete pods --as dev-user
```
The role can be used to specify
- Resources
- Namespaces
- ResourceNames

```yaml
kind: Role
Metadata:
  Name: developer
Rules:
- apiGroups: [""]
      Resources: ["pods"]
      Verbs: ["get", "create", "update"]
      resourceNames: ["blue", "orange"]
```
```
kubectl create role developer --verb list --verb create --verb delete --resource pods
role.rbac.authorization.k8s.io/developer created

kubectl create rolebinding dev-user-binding --role developer --user dev-user
rolebinding.rbac.authorization.k8s.io/dev-user-binding created
```
```yaml
kind: Role
metadata:
  creationTimestamp: "2025-08-28T06:54:34Z"
  name: developer
  namespace: blue
  resourceVersion: "2867"
  uid: 97cda8f8-0cef-4106-ad72-43a820e2b734
rules:
- apiGroups:
  - apps
  resourceNames:
  - dark-blue-app
  resources:
  - pods
  verbs:
  - get
  - watch
  - delete
  - list
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - create
```

### Cluster role
```
kubectl create clusterrole access-node --verb "*" --resource nodes
kubectl create clusterrolebinding access-node-michelle --user michelle --clusterrole access-node 

kubectl create clusterrole storage-admin --resource persistentvolumes,storageclasses --verb "*" 
kubectl create clusterrolebinding michelle-storage-admin --user michelle --clusterrole storage-admin 
```

##

### Service Accounts

- Every service account should have a token associated with it.
- The token can be used to access the kube api server.
- Every namespace has a default service account.
- Every pod in a namespace can get the token of the default service account from the token request api.

- The changes from the token request api happened in 1.22 and 1.24.
```
kubectl exec -it web-dashboard-5f88cdc488-lwdzr -- sh

kubectl create token dashboard-sa
```

Disable automountServiceAccountToken is a best security practice.

```yaml
automountServiceAccountToken: false
```

##

### Image Security

```yaml
Image: docker.io/library/nginx

Image: <registry>/<user/account>/<image/repository>
```

For private repository
```
kubectl create secret docker-registry regard --docker-server my.io --docker-username my --docker-password my --docker-email my@org.io
```
Inside pod ...
```yaml
imagePullSecrets:
- ame: regcred
```
### Docker security

### Security Context

- Linux capabilities
- User id

```yaml
securityContext:
  runAsUser: 10000
  Capabilities:
    Add: ["MAC_ADMIN"]
```

##

### Network Policies
```yaml
policyTypes:
- Ingress
- Egress

ingress:
  - from: 
    - podselector:
        matchLabels:
          name: api-pod
      namespaceSelector: #w/o dash -> and with dash -> or
        matchLabels:
          name: prod
    - ipBlock:
        cidr: 192.168.2.12/32
           
    Ports:
      - protocol: TCP
        port: 3306
Egress:
  - to:
    - ipBlocl:
        cidr: 192.168.3.13/32
    Ports:
    - protocol: TCP
      port: 80
```
    
Flannel does not support Network Policies

policyTypes:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```

##

### Custom resource definition

flightticket.yaml
```yaml
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: my-flight-ticket
spec:
  from: Mumbai
  to: London
  number: 2
```
flightticket-custom-definition.yaml
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata: 
  name: flight tickets.flights.com
spec: 
  scope: Namespaced
  group: flights.com
  names: 
    kind: FlightTicket
    singular: flightticket
    plural: flighttickets
    shortNames:
      - ft
  verions: 
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema: 
          type: object
          properties:
            spec: 
              type: object
              properties: 
                from:
                  type: string
                to:
                  type: string
                number:
                  type: string
```
https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/

```yaml
kind: Global
apiVersion: traffic.controller/v1
metadata:
  name: datacenter
spec:
  dataField: 2
  access: true
```

##

### Custom Controllers

https://github.com/kubernetes/sample-controller

### Operator Framework

https://operatorhub.io/

### Docker storage

/var/lib/docker/volumes
Layered architecture

The image layers are r/o and the container layer is rw.
There is copy-on-write to modify a file from the image layer in the container layer.
To persist data we need to mount a volume.

docker run -V data_volume:/var/lib/mysql mysql

- volume mount
- bind mount

docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql #new way to mount

Storage drivers
- Aufs
- Zfs
- Brefs
- Device mapper
- Overlay
- Overlay 2

### Volume driver plugins in docker
```
docker run -it --name mysql --volume-driver rexray/ebs --mount src=ebs-vol,target=/var/lib/mysql mysql
```

### Container storage interface

### Volumes

### Persistent Volumes

PV (admin)
PVC (users)

Access mode
- ReadOnlyMany
- ReadWriteOnce
- ReadWriteMany
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes: 
    - ReadWriteOnce
  Capacity: 1G
#  hostPath:
#    Path: /tmp/data
   awsElasticBlockStore:
      volumeID: xxx
      fsType: ext4
```
### PersitentVolumeClaims

One to one relation between PVC and PV !!!

```yaml
apiVersion: v1
kind PersitentVolumeClaim
metadata:
  Name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  Resources:
    Requests: 
      Storage: 500Mi 
```
persistemtVolumeReclaimPolicy: (what happens to the volume when the pvc is deleted) 
  - Retain : not deleted but not available, usefull to protect data !!!
  - Delete
  - Recycle : deprecated replaced by delete
 

### Storage class

Dynamic provisioning.

You define a provisioner that will automatically provision storage and attach to pods when the claim is made.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google-storage
provisioner: kubernetes.io/gce-pd
```

PV will be created automatically.
We specify the SC in the PVC.

##

### Linux networking basics

##

### DNS

```
/etc/hosts

/etc/resolv.conf
nameserver x.x.x.x
search payplug.com

/etc/nsswitch.conf

Record types
A web-server x.x.x.x
CNAME food.webserver eat.server

nslookup or dig #do not consider host file
```

##

### Network namespaces

TOREDO: RELISTEN BEST GLOBAL NETWORK EXPLANATION !!! !!! !!!
```
ip netns add red
ip netns add blue

ip netns

ip link

ip nets exec red ip link #execute ip link inside the ns red

#With a network ns we prevent the container to see the host interfaces.
#This is the same with arp tables ...

ip nets exec red arp 

#We may use open vSwitch to connect ns.
```
### Docker networking

- None
- Host
- Bridge : internal private network 

```
docker network ls
```
docker0 on the host is the bridge

Container = network namespace

Container -> namespace -> link -> bridge network

Docker port mapping is done with iptables native.

1. Create namespace
2. Create bridge network/interface
3. Create VETH pairs (virtual cable)
4. Attach vEth to namespace
5. Attach other vEth to Bridge
6. Assign IP addresses
7. Bring the interfaces up
8. Enable NAT - IP masquerade

### CNI

Docker does not implement CNI but CNM.

### Networking cluster node.

Commands to use ... !!! !!! !!!

```
ip link
ip addr
ip addr add 192.168.1.10/24 dev eth0
ip route
route
ip route add 192.168.1.0/24 via 192.168.2.1
cat /proc/sys/net/ipv4/ip_forward
arp
netstat -pnlt
netstat -anp
netstat -anp | grep etcd | grep 2380 | wc -l
```
##
### Pod networking

??? TO RE READ IF NEEDED ???

##
### CNI in kubernetes

##
CNI Weave
```
ls -al /opt/cni/bin
cat /etc/cni/net.d/10-flannel.conflist 
```
##
IPAN CNI
```
rm /etc/cni/net.d/10-flannel.conflist
```

https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/tigera-operator.yaml

curl https://raw.githubusercontent.com/projectcalico/calico/v3.29.3/manifests/custom-resources.yaml -O

#modify cidr range

kubectl apply -f custom-resources.yaml 

watch kubectl get pods -A
```
##
### Service networking

Is implemented through iptables and NAT rules on the nodes by kube-proxy.
The service is so translated into ip node and port.
The service is available across all nodes in a virtual way.

```
ps auxf | grep controller

Pod range:
--cluster-cidr=172.17.0.0/16

Service range:
--service-cluster-ip-range=172.20.0.0/16
```
##
### Cluster DNS

For service
```
curl http://web-service
curl http://web-service.apps
curl http://web-service.apps.svc
curl http://web-service.apps.svc.cluster.local
curl http://<service>.<namespace>.svc.cluster.local
```
For pods disable by default in Corefile
```
curl http://10-244-2-5
curl http://10-244-2-5.apps
curl http://10-244-2-5.apps.pod
curl http://10-244-2-5.apps.pod.cluster.local
curl http://<IP>.<namespace>.pod.cluster.local
```
##
### CoreDNS in kubernetes

From 1.20 kubeDNS -> CoreDNS

2 pods

cat /etc/coredns/Corefile
-> kubectl get cm -n kube-system

Kubelet configure nameserver for CoreDNS

The service is kube-dns by default.


forward: A global rule for all external DNS queries.
stubDomains: A specific rule for a particular domain.

##
### Ingress
```
kubectl create ingress --help
```
apiVersion: networking.k8s.io/v1


##
### Gateway api

- GatewayClass
- Gateway
- HttpRoute

Gateway is an implementation of GatewayClass.

GatewayClass <- Gateway <- HttpRoute

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: GatewayClass
metadata:
  name: nginx
spec:
  controllerName: nginx.org/gateway-controller
---
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: nginx-gateway
  namespace: default
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    protocol: HTTP
    port: 80
    allowedRoutes:
      namespaces:
        from: All
---
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: basic-route
  namespace: default
spec:
  parentRefs:
  - name: nginx-gateway
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /app
    backendRefs:
    - name: my-app
      port: 80
```
```
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v1.5.1" | kubectl apply -f -
kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/crds.yaml
kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/nodeport/deploy.yaml
kubectl get pods -n nginx-gateway
kubectl get svc -n nginx-gateway nginx-gateway -o yaml
```
##

A gateway can accept routes from all namespaces
```yaml
# Create the Gateway
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: web-gateway
  namespace: nginx-gateway
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    port: 80
    protocol: HTTP
    allowedRoutes: # -> !!!
      namespaces:
        from: All
```
##

### Migrate from ingress to gateway

1. Install a gateway controller

Nginx, contour, Istio, ...

2. Create the gateway resource
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: my-gateway
spec:
  gatewayClassName: my-gateway-class # from gateway controller
  listeners:
  - name: http
    protocol: HTTP
    port: 80
    hostname: "example.com"
```
3. Create the HttpRoute
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: my-app-route
spec:
  parentRefs:
  - name: my-gateway # from gateway
  hostnames:
  - "example.com"
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /
    backendRefs:
    - name: my-service
      port: 80
```
##
```
kubectl create -n default -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web-route
  namespace: default
spec:
  parentRefs: # do not forget parentRefs referencing gateway
    - name: web-gateway
      namespace: default
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: web-service
          port: 80
          weight: 80
        - name: web-service-v2
          port: 80
          weight: 20
EOF
```
##
### Design a Kubernetes cluster

##
### Choosing kubernetes infrastructure
 
##
### Configure HA

##
### ETCD in HA

##
### Introduction to deployment

##
### KUBEADM
Cluster installation
```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.33.0-1.1' && \
sudo apt-mark hold kubeadm

sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.33.0-1.1' kubectl='1.33.0-1.1' && \
sudo apt-mark hold kubelet kubectl

systemctl daemon-reload
systemctl restart kubelet
```

##

Install a Debian package
```
dpkg -i ...

systemctl start cri-docker
systemctl enable cri-docker
```

## 

## Helm
What is helm

##
### Installation and configuration of helm

##
### A quick note about Helm 2

3 way strategic merge patch
- Previuos release 
- Current release 
- Live state

##
### Helm components

- Helm cli
- Charts
- Release
- Revision
- Chart repository
- Metadata as a kubernetes secret

Helm charts
- Values
- Templating
- Release #differente releases of the same chart

Helm repositories
- Artifacthub.io
- 6311 packages

##
### Helm charts
```
helm install <release> <repo/chart>

values.yaml
Chart.yaml
```
apiVersion:
Helm 2 v1
Helm 3 v2

Types:
- Application
- Library

Dependencies
-> so need to add the dependency helm ...

##
### Working with helm
```
helm search hub repo

helm list #list the releases
helm uninstall <release>

helm repo list 
helm repo update #refresh the repo locally
```
##
### Customize chart parameters
```
helm install --values custom-values.yaml my-release bitnami/wordpress

helm pull bitnami/wordpress #creates a directory
#modify in the directory ./wordpress
helm install my-release ./wordpress
```
##
### Helm lifecycle management
```
helm history nginx-release
helm rollback nginx-release 1 #creates revision 1 ??? 3
```
You may need to use charts hooks if you need to upgrade something that is external to kubernetes.
```
helm upgrade dazzling-web bitnami/nginx --version 18.3.6
```

##

### Helm install w/o CRDs 

It will skip the installation of the CRDs contained in the crds/ folder.
You can do it by using --skip-crds

##
```
helm ls -A
helm repo ls
helm search repo kk-mock1/nginx
helm upgrade kk-mock1 kk-mock1/nginx -n kk-ns --version=18.1.15
```

##

### Use of helm template
```yaml
{{ .Values.myValue | quote }}: Wraps the value in quotes.
{{ .Values.myValue | upper }}: Converts text to UPPERCASE.
{{ .Values.myValue | default "blue" }}: Uses "blue" if myValue is missing.

#in values.yaml
envVars:
  - name: ENV_TYPE
    value: production
  - name: DB_HOST
    value: localhost

#-> 

#in templates/deployment.yaml
env:
  {{- range .Values.envVars }}
  - name: {{ .name }}
    value: {{ .value | quote }}
  {{- end }}
```

##

### Some helm commands.

```
helm repo add nginx-stable https://helm.nginx.com/stable
helm search repo nginx # serach chart nginx !!! 
helm show chart nginx-stable/nginx-ingress
helm show values nginx-stable/nginx-ingress
helm get manifest my-nginx-ingress -n nginx-ingress
helm history my-nginx-ingress -n nginx-ingress
helm rollback my-nginx-ingress 3 -n nginx-ingress

```

##

## Kustomize

### Kustomize problem statement

- Base
- Overlays
```
k8s/
|- base/
|  |- kustomization.yaml
|  |- nginx-depl.yaml
|  |- service.yaml
|  |- redis-depl.yaml
|--overlays/
   |- dev/
   |  |- kustomization.yaml
   |  |- config-map.yaml
   |- stg/
   |  |- kustomization.yaml
   |  |- config-map.yaml
   |- prod/
      |- kustomization.yaml
      - config-map.yaml
```
You may need to update kustomize to get the latest.

##
### Kustomize vs Helm

##
### Install kustomize

##
kustomization.yaml
```yaml
resources:
  - ngnix-depl.yaml
  - nginx-service.yaml

commonLabels:
  company: KodeKloud
```
kustomize build k8s/ #outputs to terminal does not apply

##
### Kustomize output
```
kustomize build k8s/ | kubectl apply -f -
#or
kubectl apply -k k8s/

kustomize build k8s/ | kubectl delete -f -
#or
kubectl delete -k k8s/
```

##
### Managing directories

k8s/kustomization.yaml

```yaml
---
resources:
  - api/
  - db/
  - cache/
  - kafka/
```
k8s/db/kustomization.yaml
```yaml
---
resources:
  - db-depl.yaml
  - db-service.yaml
```
##
### Common Transformers

- commonLabel
- namePrefix
- nameSuffix
- Namespace
- commonAnnotations

###
### Image transformers

kustomization.yaml
```yaml
---
images:
  - name: nginx
    newName: haproxy
    newTag: "2.4"

->

...
    spec:
      containers:
        - name: web
          image: haproxy:2.4


apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
```
https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/


##
### Patch intro

Types
- add 
- remove
- patch

Target
- kind
- version/group
- name
- namespace
- labelSelector
- annotationSelector

Value

### Json 6902 patch

kustomization.yaml
```yaml
---
patches:
  - target:
      kind: Deployment
      name: api-deployment

    patch: |-
      - op: replace
        path: /metadata/name
        value: web-deployment

      - op: replace
        path: /spec/replicas
        value: 5
```

### Strategic merge patch

kustomization.yaml
```yaml
---
patches:
  - patch: |-
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: api-deployment
      spec:
        replicas: 5
```
##
### Different types of patches

##
### Patches dictionary

TODO: get the dictionary in yaml ??? YES TODO

##
### Patches list

Replace Json6902 

kustomization.yaml
```yaml
---
patches:
  - target:
      kind: Deployment
      name: api-deployment
    patch: |- 
      - op: replace
        path: /spec/template/spec/containers/0
        value:
          name: haproxy
          image: haproxy
```
Replace Strategic merge patch

kustomization.yaml
```yaml
--- 
patches:
  - label-patch.yaml

label-patch.yaml
---
apiVersion: app/v1
kind: Deployment
metadata: 
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: nginx
          image: haporxy
```
##
### Add Json6902

kustomization.yaml
```yaml
---
patches:
  - target:
      kind: Deployment
      name: api-deployment
    patch: |- 
      - op: add
        path: /spec/template/spec/containers/-
        value:
          name: haproxy
          image: haproxy
```

### Add strategic merge patch

kustomization.yaml
```yaml
--- 
patches:
  - label-patch.yaml

label-patch.yaml
---
apiVersion: app/v1
kind: Deployment
metadata: 
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: haproxy
          image: haporxy
```


### Delete list Json6902

kustomization.yaml
```yaml
---
patches:
  - target:
      kind: Deployment
      name: api-deployment
    patch: |- 
      - op: remove
        path: /spec/template/spec/containers/1
        value:
          name: haproxy
          image: haproxy
```

### Delete list strategic merge patch

kustomization.yaml
```yaml
--- 
patches:
  - label-patch.yaml

label-patch.yaml
---
apiVersion: app/v1
kind: Deployment
metadata: 
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - $patch: delete
          name: haproxy
```
          
##
### Overlays

##
### Components

```
k8s/
|- base/
|  |- kustomizayion.yaml
|  |- api-dev.yaml
|- components/
|  |- caching/
|  |  |- kustomization.yaml
|  |  |- deployment-patch.yaml
|  |  |- redis-depl.yaml
|  |- db/
|  |  |- kustomization.yaml
|  |  |- deployment-patch.yaml
|  |  |- postgres-depl.yaml
|- overlays/
   |- dev/
   |  |- kustomization.yaml
   |- premium/
   |  |- kustomization.yaml
   |- standalone/
   |  |- kustomization.yaml
```

Components/db/kustomization.yaml
```yaml
---
apiVersion: kustomize.config.k8s.io/v1alpha1
kind: component
resources:
  - postgres-depl.yaml
secretGenerator:
  - name: postgres-cred
    literals: 
      - password=postgress123
patches:
  - deployment-patch.yaml
```

Overlay/dev/kustomization.yaml
```yaml
---
bases:
  - ../../base
components:
  - ../../components/db
```

##

### Use expose to create a service.

```
kubectl expose deployment hr-webapp --type=nodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml
#Then vi and change the nodePort
```

kubectl apply -k "https://github.com/kubernetes-sigs/kustomize/releases/latest/download/kustomize-crds.yaml"


##
### Json path

```json
$[0] #the first element of the root array

$[0,3] #the first and the fourth elements of the root array

# [ for an array
# . for the dictionary

$.car.wheels[1].model

# ?() for a criteria or filter
# @ for each item in the list

$[?( @ > 40 )]

$.car.wheels[?(@.location == "rear")].model

cat q13.json | jpath '$[0,3]'

$.*.color

$[*].model

$.*.wheels[*].model

cat q9.json | jpath '$.prizes[*].laureates[*]'
cat q9.json | jpath '$.prizes[*].laureates[?(@.id==913)]'

$[0:3] #the first faout elements
$[0,3] #the first and the fourth
$[0:8:2] #by step
$[-1] #the last
$[-1:0] #the last
$[-3:] #the 3 last elements
```
 
##

### Json Path for kubernetes
```
kubectl get pods -o=jsonpath'{ .items[0].spec.containers[0].image }'
```
- No need for $, added by kubectl
- Command must be included in '{ }'
```
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

```

##
##

## EXAM TIPS AND TRICKS

##

tips for searching the documentation

1. search for kind: <resource>
2. search for unique fields: readinessProbe or nodeSelector
3. Know if you need to search in concepts or tasks
4. Avoid Reference: the api is ugly
5. kubectl cheat sheet
6. Concepts / Workloads / Workload management


##

### How to get apiVersion and kind

```
kubectl api-resources
# get all kubernetes resources

kubectl api-versions
# get all supported api versions

kubectl explain networkpolicy --recursive=false

kubectl explain networkpolicy --recursive=false
GROUP:      networking.k8s.io
KIND:       NetworkPolicy
VERSION:    v1

->

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
spec:
  # add your specifications here

#OK
```

### kubectl completion

```
#ensure kubectl completion is enabled in kubectl quick reference ...

source <(kubectl completion bash) # set up autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```
##


### crictl 

can be installed by apt on the node or by downloading the binary from github

https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/

- crictl pods	    Lists all the pod sandboxes on the node.	                  
                              crictl pods
- crictl ps	      Lists all containers, and you can add -a to show all containers, including stopped ones.	crictl ps -a

- crictl images	  Lists all images available on the node.	                                                  crictl images

- crictl pull	    Pulls an image from a container registry.	                                                crictl pull nginx:latest

- crictl logs	    Fetches the logs of a specific container.	                                                crictl logs <container-id>

- crictl exec	    Runs a command inside a running container.	                                              crictl exec -it <container-id> sh

- crictl inspectp	Displays detailed status information for one or more pods.	                              crictl inspectp <pod-id>

- crictl inspect	Displays detailed status information for one or more containers.	                        crictl inspect <container-id>

##

### Call the kubernetes API using a secret mounted in a pod

1. Create a service account
2. Create a role
```yaml
rules:
- apiGroups: [""]
  resources: ["pods/log"]
  verbs: ["get", "watch", "list"]
```
3. Create a RoleBinding between the service account and the role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: my-pod-reader-binding
  namespace: default
subjects:
- kind: ServiceAccount
  name: my-service-account
  namespace: default
roleRef:
  kind: Role
  name: my-pod-reader-role
  apiGroup: rbac.authorization.k8s.io
```

##

### New last tips and tricks

The `--previous` flag with `kubectl logs` is specifically designed to retrieve logs from a previous instance of a container that has crashed and restarted.

A `NewReplicaSetAvailable` reason with a `Progressing` status of `False` can be a confusing symptom. It often occurs when the Pod template has not actually changed in a way that triggers a new `ReplicaSet`, such as when the `imagePullPolicy` is set to `IfNotPresent` and the old image tag is reused, preventing a new image from being pulled and the new `ReplicaSet` from becoming available.

An `emptyDir` volume is the standard method for sharing a temporary directory between containers in a multi-container Pod. It is created on the node's disk when the Pod is created and is deleted when the Pod is removed.

##

### CM and Secrets

Kubernetes automatically updates a Pod's mounted `ConfigMap` volume with a slight delay (typically on the order of a minute) after the `ConfigMap` resource is updated. This allows the application to read the new configuration without a Pod restart.

Updating a Kubernetes Secret has different effects depending on how the secret is used by a pod.
#### Secret Mounted as a Volume (wait 2 minues)
When a secret is mounted as a volume in a pod, changes to the secret are automatically updated in the pod's file system.
This update is not instantaneous; it can take up to a minute or two for the changes to propagate. You won't need to restart the pod for it to see the new secret data.
The pod will be able to read the new files from the mounted directory.
#### Secret Used as an Environment Variable (reload needed)
When a secret is used to set an environment variable, updating the secret does not automatically update the environment variable inside the running pod
The environment variables are set only when the pod is created. To get the new secret value, you must restart or recreate the pod.
This is because environment variables are static and are not re-evaluated after the pod has started.
#### Secret Used from a Secret Key Reference (reload needed)
Similar to environment variables, a secret referenced via secretKeyRef in a pod's spec to provide a single value for a field is not automatically updated.
The value is fetched when the pod is created, and it remains static for the pod's lifetime.
To get the new secret value, you must restart or recreate the pod.
This applies to various fields where a secret reference might be used, such as container arguments or command.

By design, Kubernetes secrets are namespaced and cannot be accessed from a Pod in a different namespace for security reasons.
The only way for a Pod to use a secret from another namespace is to create a copy of that secret in its own namespace.
I knew it as i use to do copy from one ns to another ns.

Using a dedicated external secrets manager and mounting secrets at runtime via a CSI driver is considered a best practice for strong separation of concerns and securing secrets outside of the cluster's etcd.
Is considered better than mounting a secret as a volume.


##

### StatefulSet

A `StatefulSet` uses a `Headless Service` (a Service with `clusterIP: None`) to provide a stable, unique DNS entry for each Pod replica.
This, combined with the Pod's stable name, gives it a persistent network identity.
-> Stable Network Identifiers: A StatefulSet, along with a Headless Service, creates stable network identities for each Pod.
The Headless Service doesn't have a cluster IP. Instead, it creates DNS records for each Pod, allowing other services to discover
and connect to them individually (e.g., web-0.nginx-service.default.svc.cluster.local).
-> Persistent Storage: A StatefulSet typically uses a volumeClaimTemplate to provision PersistentVolumeClaims (PVCs) for each Pod.
Each Pod gets its own dedicated PVC, ensuring its data is persistent and not shared with other Pods. This is vital for stateful applications like databases
where each replica needs its own storage. The PVCs and their associated PersistentVolumes (PVs) are not deleted when the StatefulSet or Pods are removed, preserving the data.

##

### Storage modes.

ReadWriteOnce     (RWO)   #once by node

ReadWriteOncePod  (RWOP)  #once by pod

ReadWriteMany     (RWX)

ReadOnlyMany      (ROX)

##

### NodeName

Setting the `NodeName` field in a Pod's spec directly tells the `kubelet` on that specific node to create the Pod, effectively bypassing the `kube-scheduler`.

##

KCSA

Setting `runAsNonRoot: true` ensures the container runs with a non-root user ID, and `readOnlyRootFilesystem: true` prevents the container from writing to its own filesystem.


##

Security


The main security benefit of a `scratch` image is its minimal attack surface, as it doesn't contain a shell or operating system files that could be exploited.

Docker-shim was a component within the Kubernetes kubelet that acted as an adapter, allowing the Kubernetes control plane to use the Docker Engine as a container runtime.
kubelet -> docker-shim -> Docker Daemon -> containerd -> runc

hostIPC: true

By sharing the host's IPC namespace, a container can potentially send signals to or read shared memory from other processes on the host, which is a significant security risk.

Linux capabilities are a security mechanism that breaks up the all-powerful root privilege into smaller, distinct units.

By default, `/tmp` is part of the root filesystem. However, in many container runtimes, `/tmp` is a symlink or a memory-based filesystem, so this is a trick question. 
The CKS exam expects you to know that if `/tmp` is a `tmpfs` volume, it will succeed, otherwise it will fail.

The `execve` syscall is used to execute a program, so denying it will prevent the container from creating new processes. This is a very common scenario in CKS exams.

Mounting a volume to the `/var/log` directory is a common pattern to allow a container to write to a specific location while the rest of the filesystem remains read-only.

A custom `Seccomp` profile is a JSON file that defines a specific set of allowed syscalls, which is a common practice in the CKS exam.

##

valueFrom

valueFrom is used to set the value of an env var.
It could be: 
- secretKeyRef: from a secret
- configMapKeyRef: from a CM
- fieldRef: from the pod itself
- resourceFieldRef: from the container's resource

##

Multicontainer

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mc-pod
  namespace: mc-namespace
spec:
  containers:
    - name: mc-pod-1
      image: nginx:1-alpine
      env:
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
    - name: mc-pod-2
      image: busybox:1
      volumeMounts:
        - name: shared-volume
          mountPath: /var/log/shared
      command:
        - "sh"
        - "-c"
        - "while true; do date >> /var/log/shared/date.log; sleep 1; done"
    - name: mc-pod-3
      image: busybox:1
      command:
        - "sh"          #a tip for multiple commands
        - "-c"
        - "tail -f /var/log/shared/date.log"
      volumeMounts:
        - name: shared-volume
          mountPath: /var/log/shared
  volumes:
    - name: shared-volume
      emptyDir: {}      #shared not persistent
````

##

Install cri docker

```
dpkg -i /root/cri-docker_0.3.16.3-0.debian.deb
systemctl start cri-docker
systemctl enable cri-docker
systemctl is-active cri-docker
systemctl is-enabled cri-docker
systemctl daemon-reload #may be needed
```

##

Install with kubeadm
```
vim /etc/apt/sources.list.d/kubernetes.list #master and node

deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /

#master

#change the package repository

kubectl drain controlplane --ignore-daemonsets #not in doc for control plane !!!
apt update
apt-cache madison kubeadm #get the full version

apt-get install kubeadm=1.33.0-1.1

kubeadm upgrade plan v1.33.0
kubeadm upgrade apply v1.33.0

apt-get install kubelet=1.33.0-1.1
systemctl daemon-reload
systemctl restart kubelet
kubectl uncordon controlplane

# Identify the taint first. 
kubectl describe node controlplane | grep -i taint #not in doc !!!

# Remove the taint with help of "kubectl taint" command.
kubectl taint node controlplane node-role.kubernetes.io/control-plane:NoSchedule-

# Verify it, the taint has been removed successfully.  
kubectl describe node controlplane | grep -i taint

#node

kubectl drain node01 --ignore-daemonsets
apt update
apt-cache madison kubeadm #get the full version

apt-get install kubeadm=1.33.0-1.1
# Upgrade the node 
kubeadm upgrade node

apt-get install kubelet=1.33.0-1.1
systemctl daemon-reload
systemctl restart kubelet

kubectl uncordon node01
kubectl get pods -o wide | grep gold # make sure this is scheduled on a node

#change the package repository

export ETCDCTL_API=3
etcdctl snapshot save --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=127.0.0.1:2379 /opt/etcd-backup.db
```

##

Volumes

The `Retain` policy ensures the underlying storage is not deleted. The PV object transitions to a `Released` state, requiring an administrator to manually clean it up before it can be reused.
The data remains on the volume.

##

Rollinupdate

Updating the container image tag in the pod template is the standard trigger for a rolling update, causing Kubernetes to create new pods with the new image.

During a `RollingUpdate`, for a time, pods from both the old and new ReplicaSets are running simultaneously, thus consuming more resources than `Recreate`, which terminates old pods before creating new ones.

##

Etcdctl

etcdctl snapshot save
etcdctl snapshot restore

To restore the snapshot to a new `etcd` instance, you must have the same TLS certificates and keys that were used to secure the original cluster.

##

The `endpointslice-controller`, which runs inside the `kube-controller-manager`, is responsible for creating and managing `EndpointSlices` based on the pods that match a service's selector.

##

The API server is configured with a Certificate Authority (CA). Any client certificate signed by this CA is considered authentic.

##

This command correctly uses the `--copy-to` flag to create a new pod named `my-pod-debug` as a copy of the original and attaches an interactive terminal.
kubectl debug my-pod -it --copy-to=my-pod-debug --image=busybox -- /bin/sh
This will create a copy of my-pod but replace the container's image with busybox, which is a lightweight image that includes common debugging tools.

##

`kubeadm` enables automatic certificate renewal for control plane components as part of the `kubeadm upgrade` process. Running this command manually is an alternative but not the automatic mechanism.

##

`NetworkPolicy` resources are just objects in the API server; a CNI plugin like Calico, Cilium, or Weave Net is required to actually implement and enforce the firewall rules they define.

##

The kubelet is a client of the API server and, like `kubectl`, uses a kubeconfig file (often located at `/etc/kubernetes/kubelet.conf`) to find the server's endpoint and credentials.

##

VPA !!! !!! !!!
```
kubectl create -n default -f - <<EOF
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: analytics-vpa
  namespace: default
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: analytics-deployment
  updatePolicy:
    updateMode: "Auto"
EOF
```
or ...
```
kubectl explain vpa
GROUP:      autoscaling.k8s.io
KIND:       VerticalPodAutoscaler
VERSION:    v1
...
->
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata: 
  name: vpa
  namespace: default
```

##

5
create a user, csr, role, ...
```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUt2Um1tQ0h2ZjBrTHNldlF3aWVKSzcrVVdRck04ZGtkdzkyYUJTdG1uUVNhMGFPCjV3c3cwbVZyNkNjcEJFRmVreHk5NUVydkgyTHhqQTNiSHVsTVVub2ZkUU9rbjYra1NNY2o3TzdWYlBld2k2OEIKa3JoM2prRFNuZGFvV1NPWXBKOFg1WUZ5c2ZvNUpxby82YU92czFGcEc3bm5SMG1JYWpySTlNVVFEdTVncGw4bgpjakY0TG4vQ3NEb3o3QXNadEgwcVpwc0dXYVpURTBKOWNrQmswZWhiV2tMeDJUK3pEYzlmaDVIMjZsSE4zbHM4CktiSlRuSnY3WDFsNndCeTN5WUFUSXRNclpUR28wZ2c1QS9uREZ4SXdHcXNlMTdLZDRaa1k3RDJIZ3R4UytkMEMKMTNBeHNVdzQyWVZ6ZzhkYXJzVGRMZzcxQ2NaanRxdS9YSmlyQmxVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ1VKTnNMelBKczB2czlGTTVpUzJ0akMyaVYvdXptcmwxTGNUTStsbXpSODNsS09uL0NoMTZlClNLNHplRlFtbGF0c0hCOGZBU2ZhQnRaOUJ2UnVlMUZnbHk1b2VuTk5LaW9FMnc3TUx1a0oyODBWRWFxUjN2SSsKNzRiNnduNkhYclJsYVhaM25VMTFQVTlsT3RBSGxQeDNYVWpCVk5QaGhlUlBmR3p3TTRselZuQW5mNm96bEtxSgpvT3RORStlZ2FYWDdvc3BvZmdWZWVqc25Yd0RjZ05pSFFTbDgzSkljUCtjOVBHMDJtNyt0NmpJU3VoRllTVjZtCmlqblNucHBKZWhFUGxPMkFNcmJzU0VpaFB1N294Wm9iZDFtdWF4bWtVa0NoSzZLeGV0RjVEdWhRMi80NEMvSDIKOWk1bnpMMlRST3RndGRJZjAveUF5N05COHlOY3FPR0QKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  usages:
  - digital signature
  - key encipherment
  - client auth
```

```
kubectl certificate approve john-developer

kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development

kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development

kubectl auth can-i update pods --as=john --namespace=development
```
##

Give access alice to kubernetes cluster

https://kubernetes.io/docs/tasks/tls/certificate-issue-client-csr/
KEYWORDS client csr

1. Create key.
```
openssl genrsa -out alice.key 2048
#OK
```
2. Generate alice csr.
```
openssl req -new -key alice.key -out alice.csr -subj "/CN=alice/O=devs"
#OK
```
3. Encode the csr.
```
CSR_BASE64=$(cat alice.csr | base64 -i -)
#OK
```
4. Create the csr.
```
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: alice
spec:
  request: ${CSR_BASE64}
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
  # Set a long expiration, e.g., 365 days (31536000 seconds)
  expirationSeconds: 31536000
EOF
#OK
```
5. Approve the request.
```
kubectl certificate approve alice
#OK
```
6. Get the certificate.
```
kubectl get csr alice -o jsonpath='{.status.certificate}' | base64 -o alice.crt -d -i -
#OK
```
7. Create the role
```
kubectl create role pod-reader --verb=get,list,watch --resource=pods -n default
#OK
```
8. Create the binding 
```
kubectl create rolebinding devs-pod-reader-binding --role=pod-reader --group=devs -n default
#OK
```
9. Set credentials =~ create user alice !!!
```
kubectl config set-credentials alice --client-certificate=alice.crt --client-key=alice.key --embed-certs=true
#OK
```
10. Set context
```
kubectl config set-context alice-context --cluster=kind-my-kind --user=alice
#OK
```
11. Test
```
kubectl --context=alice-context get pods -n default
#OKOKOK
```
##

DNS resolver

https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/
KEYWORDS nslookup pod
```
kubectl run nginx-resolver --image=nginx
#OK

kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP
#OK

kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
#OK

kubectl get pod nginx-resolver -o wide
#OK

kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup 192-168-116-198.default.pod
#OK

```
##

create user john
```
openssl genrsa -out john.key 2048

openssl req -new -key john.key -out john.csr -subj "/CN=myuser/O=devs"

cat john.csr | base64 | tr -d "\n"

cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john
spec:
  # This is an encoded CSR. Change this to the base64-encoded contents of myuser.csr
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1pUQ0NBVTBDQVFBd0lERVBNQTBHQTFVRUF3d0diWGwxYzJWeU1RMHdDd1lEVlFRS0RBUmtaWFp6TUlJQgpJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBM3V2RDFtYXV4WEJMZTJsV1dyVnNXWTFUCittWFJwUHJuUFN3V094Zk1vUlE2YzVGU0ZSTE4zWXlxVGZKTk9wVzBINWVCRm5jNDI1NkRZVXZxRkdocUx1cjEKRFdiL2l1dFBYOUQvMDBkUHJMbWVEWENkMWZLYStNdDJBNzA3MHpOcTJ1SjBvb3ExN0JsMTBmTkhYK0xZbDJ5NwoxUXhGdi8xZ0thUEh2WGNoUW9BdTdQalBDdlJOak1vbEoyOTJtMS9YRnVXYytGcXJiNEpiNFpEenQyQkdmZ0xwCkQ3WThhV0ZYSi9YSm1IT0w3U1B4dkVVSXMrWWpMZjQvbjFNQUZNdjNqV0ZvOGZOaytIWndIb3dYRDl3elhJbGgKMWI5YVJUNjBrVVNqR1J5RjQ3VThhc3FCUFdBYWNVblVucS81THpXM1UyT0lwZ0ZvZEU0NU1EdU9GRzRsOVFJRApBUUFCb0FBd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFMR2R4UlE5NHV6RmxnYTB0MmZkOUYwSU1QTzF1NDZGClBiZ1FZdWIrMm1XQ2pTRkhINkV5K1lMd2lVSldiWmNxTHl2UFZ3THZxalB6TG5ldmt0elhteGEzSFc1eGw1TDUKZkZ3bnFuRXY0anlKWGZBdlQralBsdWV4WE9WeGdyOEFMWndmMkVVL1U3Z3FCMGhPM1NQdEsyUGhvRjZzM1p1VApIb1V5RXg1cS9ZRlllTGR0N3lIMGFuT1FwcERDT3pOL2VTalNHLzJhbTNJY05MdTdGejlidTFlQldRMHYxa3Q5CkFaMTdKSmE5UTBGTVgybmV3OFRXNFQ4UTVidUtVQkFMUGwvQlJtSmlNNDFZa2o4ejNJd0F0VGs5amllRXFpTDEKeWhNTldiSVduV0o3R2JlS29sN0x2cndtaUdLeVcwQXNPMHV1T3pMNnR1TTd6UFhPWlg4NEtPbz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF

kubectl certificate approve john

kubectl get csr john -o jsonpath='{.status.certificate}'| base64 -d > john.crt

kubectl config set-credentials john --client-key=john.key --client-certificate=john.crt --embed-certs=true

kubectl create role developer --verb=create --verb=get --verb=list --verb=update --verb=delete --resource=pods
```

##

Kubernetes debug and investigation tips and tricks.
```
kubectl get events --sort-by=.metadata.creationTimestamp
#OK in kubectl cheat sheet

kubectl get events --all-namespaces --sort-by=.metadata.creationTimestamp
#OK in kubectl cheat sheet

kubectl logs <pod-name> -c <container-name>
#OK in kcs

kubectl exec -it <pod-name> -- sh
#OK in kcs

kubectl exec -it <pod-name> -c <container-name> -- sh
#OK in kcs

#If service is unreachable
kubectl get endpoints <service-name>
#OK

#If endpoints are missing
kubectl describe svc <service-name>
kubectl get pods --show-labels
#OK

#If a pod cannot reach another by its name
kubectl exec -it <pod-name> -- nslookup <other-pod-name>
#OK

#If a pod is being killed
kubectl describe pod <pod-name> #and check for Requests and Limits
#OK

#If pods cannot communicate
kubectl describe networkpolicy
#OK

#If a user, pod, script is getting Forbidden
kubectl auth can-i <verb> <resource> --as=<user/sa-name>
#OK

#If a container fails due to permissions
kubectl describe pod <pod-name> #and check for SecurityContext : runAsUser, fsGroup, seLinuxOptions, readOnlyRootFilesystem, allowPrivilegeEscalation, ...
#OK
```

Follow the Logs and Events Chain: Never just look at the Pod status. If it's Pending, check the Events. If it's CrashLoopBackOff, check the Logs. If the logs are silent, check the describe output's events again for probe failures or mounting issues.

Use Descriptive Labels: Good labeling is essential for isolating and debugging. Use labels for app, environment, and version to target specific resources with kubectl -l <key>=<value>.

Isolate the Failing Pod: If you suspect a specific Pod is causing issues, temporarily remove its selector label with kubectl label pod <pod-name> app=debug-broken --overwrite. This detaches it from the Service, allowing you to debug it live without affecting user traffic.

The "Ephemeral Container" Trick (v1.25+): Use kubectl debug to inject a temporary "ephemeral container" (e.g., a busybox or netshoot image with debugging tools) into an existing running Pod's namespace. This lets you debug a running Pod without restarting it, especially useful for networking problems.
```
#If static pod failure
ssh <node-name>
cat /etc/kubernetes/manifests/<pod-name>.yaml
#OK

#If kubelet failure
ssh <node-name>
systemctl status kubelet
journalctl -u kubelet
#OK

#If pod is failing due to permissions
use securityContext at the Pod and Container levels to set runAsUser, runAsGroup, or fsGroup
#OK
```
##

#### UPGRADE KUBERNETES CLUSTER WITH KUBEADM

I. Plan -> Control Plane (Master)
```
kubeadm upgrade plan
#Checks cluster health, finds recommended versions, and confirms you're ready.
```

II. Install kubeadm -> Control Plane
```
sudo apt install kubeadm=<version>
#Must be done first. Upgrade the tool itself to the target version.
```
III. Apply Control Plane -> Control Plane
```
sudo kubeadm upgrade apply v<version>
#Downloads new control plane images (API server, etcd, etc.) and updates their static Pod manifests.
```
IV. Install Kubelet/kubectl -> Control Plane
```
sudo apt install kubelet=<version> kubectl=<version>
#Upgrade the remaining binaries. This is a separate, manual step!
```
V. Restart Kubelet -> Control Plane
```
sudo systemctl daemon-reload && sudo systemctl restart kubelet
#Reloads the service to pick up the new binary version.
```
VI. Repeat -> Additional Control Plane Nodes
```
#If it's an HA cluster, repeat the process on the remaining control plane nodes.
```
VII. Worker Node Upgrade -> Worker Node
```
sudo kubeadm upgrade node
#On each worker node, runs the necessary config updates.
```
VIII. Install Kubelet/kubectl -> Worker Node
```
sudo apt install kubelet=<version> kubectl=<version>
#Upgrade the binaries on the worker node.
```
IX. Restart Kubelet -> Worker Node
```
sudo systemctl daemon-reload && sudo systemctl restart kubelet
#Restart the service on the worker node.
```
```
kubectl drain <node-name>       #-> Control Plane (Master)  Cordon (mark unschedulable) and safely evict all non-DaemonSet Pods from the node. Always use --ignore-daemonsets.
kubectl uncordon <node-name>    #-> Control Plane (Master)  Mark the node back as schedulable after the upgrade is complete.

sudo apt-mark unhold <package>  #-> All Nodes   If a package was previously held to prevent automatic updates, you must unhold it before installing the new version.
sudo apt-mark hold <package>    #-> All Nodes   Re-hold the packages after the installation to prevent unintended future updates.


# Retrieve all the images of all the pods of all the namespaces
kubectl get pods -A -o custom-columns='NAMESPACE:.metadata.namespace,POD_NAME:.metadata.name,IMAGE:.spec.containers[*].image'
#in kcs 


kubectl -n world create ingress world --rule="world.universe.mine/asia/*=asia:80" --rule="world.universe.mine/europe/*=europe:80" --class=nginx 

* -> is for pathType Prefix
class is for ingressClass

kubectl top pod --all-namespaces --sort-by=cpu --no-headers | awk 'BEGIN {OFS=","} {print $1, $2}' > high_cpu_pod.txt

edit sysctl to persist if needed
vi /etc/sysctl.conf 
sysctl -p #to apply !!!

When a pod is not resolving a service-name.namespace.svc.cluster.local the first thing to check is that on the pod the /etc/resolv.conf should point to the IP of CoreDNS service ? 

curl --insecure https://localhost:6443 from master node

kubectl run netshoot-debug --image=nicolaka/netshoot -it --rm --restart=Never -- bash

kubectl run test-client --image=busybox --restart=Never --rm -it -- wget -O- x.x.x.x:80
```

##

#### Debug Kubernetes

Debugging Kubernetes is a structured process that moves from observing high-level status down to inspecting application code and low-level cluster components. 
The best tips focus on a systematic drill-down using native kubectl commands before resorting to more complex tools.
Here is a summary of the best tips, tricks, and commands for investigating issues and debugging on a Kubernetes cluster.

🔍 The Systematic Debugging FlowAlways follow this hierarchy: 
Cluster > Node > Pod > Container (Logs/Code) > Networking.

1. Start with the Cluster and Events
Before checking your application, check the cluster's overall health.
Cluster Health Check:
Check for any unhealthy control plane components (API Server, Scheduler, Controller Manager).
kubectl get componentstatuses # Check control plane health
kubectl get nodes # Check for 'NotReady' nodes
The Golden Events Command:
Cluster events often contain the root cause of deployment, scheduling, or volume issues.
Always check these before diving into logs.
kubectl get events --sort-by=.metadata.creationTimestamp

2. Investigate the Pod Status and Configuration
The Pod is the smallest deployable unit; its state will tell you the most about application failures.
A. Pod Status & Description
Get Status:
Find the problematic Pods (look for CrashLoopBackOff, ImagePullBackOff, Pending, or Error).
kubectl get pods -n <namespace>
The Key Details Command:
This is your most valuable troubleshooting tool.
It reveals scheduling failures, failed probes, configuration errors, and termination reasons.
kubectl describe pod <pod-name> -n <namespace>

Tip: Check the Events section at the bottom for messages like FailedScheduling, FailedAttachVolume, or OOMKilled (Exit Code 137).
Tip: Check the Reason under Last State: Terminated for crashed containers.

B. Common Pod Failures
Status                Cause                                             Fix Tip
Pending               Scheduler can't place the Pod.                    Check kubectl describe pod events: Is there enough CPU/Memory on nodes? Are there |Taints/Tolerations or Node Selectors preventing placement?
ImagePullBackOff      Can't pull the container image.                   Check the image name/tag for typos, verify image exists, and ensure imagePullSecrets are configured correctly for private registries.
CrashLoopBackOffThe   application starts and then crashes repeatedly.   The best tip here is to check the previous logs. See section 3.

3. Deep Dive into the Container
Once you know which Pod is failing, look inside the container itself.
View Logs (The Application Error):
kubectl logs <pod-name> -n <namespace>

View logs from the PREVIOUS, crashed container instance**
kubectl logs <pod-name> -n <namespace> --previous

Interactive Shell (Exec): Run commands inside a running container to check configurations, file paths, or network connectivity.
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash # or /bin/sh
Ephemeral Debugging (Modern K8s): For containers without a shell or with a minimal image, use a temporary debug container attached to the same pod.Bash# Attaches a container with debugging tools to the target pod's namespace
kubectl debug -it <pod-name> --image=nicolaka/netshoot --target=<container-name>

4. Networking and Service Checks
If the Pod is running but unreachable, the issue is likely the Service or Network Policy.
Check Endpoints: Verify that the Service is selecting the correct Pods and their IPs.
kubectl get endpoints <service-name> -n <namespace>
kubectl describe service <service-name> -n <namespace>
Trick: Ensure the Service Selector matches the Pod's Labels, and the Service's targetPort matches the container's containerPort.
Test Internal Connectivity: Use kubectl exec to run curl or ping from inside a healthy Pod to the problematic Service's DNS name (<service-name>.<namespace>.svc.cluster.local).
Check DNS: Run the following command inside a Pod to ensure DNS resolution is working.
kubectl exec -it <pod-name> -- cat /etc/resolv.conf
Check Ingress: If the issue is external, check the Ingress Controller Pod logs and describe the Ingress or Gateway resources for binding errors.

5. Advanced Tools and Observability
For long-term, complex, or intermittent issues, move beyond kubectl.
Resource Monitoring (The Top Command): Identify resource bottlenecks (CPU throttling or memory pressure) that lead to performance issues or OOMKilled errors (Exit Code 137).
kubectl top pods # View pod resource usage (requires Metrics Server)
kubectl top nodes # View node resource usage
Visualization: Use a terminal UI tool like K9s for a real-time, interactive view of logs, events, and resource stats across namespaces.
Centralized Logging: Implement a stack like Prometheus/Grafana for metrics and Loki/Elasticsearch for centralized, searchable logs.
This is critical for seeing cluster-wide trends and correlating errors across microservices.
Static Analysis: Use kubectl apply --dry-run=client -f <file.yaml> to validate YAML files against the cluster's API without deploying them.
By following these steps, you can quickly and efficiently isolate whether an issue is caused by a cluster-level constraint, a misconfiguration in your Kubernetes object, or an application-level code failure.

##

ip forwarding

```
persist ip_forwarding in systemd

sysctl net.ipv4.ip_forward

vi /etc/systemd/k8s.conf

sudo sysctl -p /etc/sysctl.d/k8s.conf

sysctl net.ipv4.ip_forward
```

##

```
dpkg -i cri-dockerd_0.3.16.3-0.ubuntu-jammy_amd64.deb
sudo systemctl enable --now cri-docker.service
vi /etc/sysctl.d/k8s.conf #create the config file
net.ipv4.ip_forward=1 #with ...
sysctl -p #apply
```

##

pvc pending due to accessMode misma# vi pvc.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
  namespace: storage-ns
spec:
  accessModes:
    - ReadWriteOnce        # fix access mode to match PV object
  resources:
    ...

##

If up-to-date is 0
do
kubectl rollout resume deploy xxx

##


helm lint /opt/webapp-color-apd/

helm install webapp-color-apd -n frontend-apd /opt/webapp-color-apd


##


kubectl create -n cka24456 -f - <<EOF
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: analytics-vpa
  namespace: cka24456
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: analytics-deployment
  updatePolicy:
    updateMode: "Auto"
EOF

##

codedns config is stored in a configmap

kubectl edit cm -n kube-system coredns

##

wget https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

Edit the Flannel manifest to set the CIDR to 172.17.0.0/16:

  net-conf.json: |
    {
      "Network": "172.17.0.0/16",   #Edited
      "EnableNFTables": false,
      "Backend": {
        "Type": "vxlan"
      }
    }

kubectl apply -f kube-flannel.yml


##

Systemd - systemctl

in /etc/systemd/system/xxx.service

[Unit]
Description=
Documentation
After=postgresql.service


[Service]
ExecStart= /bin/bash /usr/bin/xxx.sh
User=project_xxx
Restart=on-failure
RestartSec=10

[Install]
WantedBy graphical.target


systemctl start xxx.service

start restart stop
enable disable

systemctl start docker
stop
restart
reload
enable
disable

systemctl daemon-reload #after installing a package

systemctl edit xxxx.service --full

systemctl get-default
systemctl set-default ...

systemctl list-units --all
systemctl list-units #only active


journalctl 
journalctl -b #for the current boot
journalctl -u docker.service

Get a list of all services associated with kubernetes.
```
systemctl list-unit-files --type service --all
systemctl list-unit-files --type service --all | grep kube
```

##

Check the readiness status of the kubernetes api server.

```
kubectl get --raw='/readyz?verbose' 
```

##

Investigation Summary

CLUSTER - NODES

```
kubectl get componentstatuses
kubectl get nodes
kubectl get events --sort-by=.metadata.creationTimestamp
```

PODS

```
kubectl get pods
kubectl describe pods|desploy|svc|...
```

Tip: Check the Events section at the bottom for messages like FailedScheduling, FailedAttachVolume, or OOMKilled (Exit Code 137).
Tip: Check the Reason under Last State: Terminated for crashed containers.

| Status               | Cause                                            | Fix Tip |
| Pending              | Scheduler can't place the Pod.                   | Check kubectl describe pod events: Is there enough CPU/Memory on nodes? Are there Taints/Tolerations or Node Selectors preventing placement? |
| ImagePullBackOff     | Can't pull the container image.                  | Check the image name/tag for typos, verify image exists, and ensure imagePullSecrets are configured correctly for private registries. |
| CrashLoopBackOffThe  | application starts and then crashes repeatedly.  | The best tip here is to check the previous logs. See section 3. |

CONTAINER

```
kubectl logs <pod-name>

kubectl exec -it <pod-name> -c <container-name> -- /bin/bash # or /bin/sh
kubectl debug -it <pod-name> --image=nicolaka/netshoot --target=<container-name>
```
NETWORK

```
kubectl get np
kubectl get all --show-labels or kubectl 
kubectl get endpoints <service-name> -n <namespace>
kubectl describe service <service-name> -n <namespace>
kubectl exec -it <pod-name> -- curl|ping <svc-name>.<namespace>.svc.cluster.local
kubectl exec -it <pod-name> -- cat /etc/resolv.conf
kubectl describe ingress|gateway
kubectl run -it --rm debug --image=busybox --restart=Never -- sh
# Inside the pod:
wget <service-name>:<port>
````

ADVANCED

```
kubectl top pod
kubectl top nodes
kubectl apply -f <file.yaml> --dry-run=client 

sudo systemctl status containerd

# On the affected node (via SSH):
sudo systemctl status kubelet
sudo journalctl -u kubelet --since "5m" -e
```

##

This is a good example of using args to pass shell code.

```yaml
spec:
  volumes:
    - name: logs-volume
      emptyDir: {}
  containers:
    - name: main-application
      image: busybox:latest
      command: ["/bin/sh", "-c"]
      args:
        - |
          while true; do
            echo "$(date) - Application event occurred." >> /var/log/app/app.log
            sleep 5
          done
      volumeMounts:
        - name: logs-volume
          mountPath: /var/log/app/
```

##


How to debug with crictl

1. Find the container runtime endpoint.

docker context ls

| NAME              | DESCRIPTION                              | DOCKER ENDPOINT |                                  
| colima            | colima                                   | unix:///Users/fabien/.colima/default/docker.sock |
| default           | Current DOCKER_HOST based configuration  | unix:///var/run/docker.sock |
| desktop-linux *   | Docker Desktop                           | unix:///Users/fabien/.docker/run/docker.sock |

2. Configure crictl
```
# Example for containerd
export CONTAINER_RUNTIME_ENDPOINT=unix:///var/run/containerd/containerd.sock
export IMAGE_SERVICE_ENDPOINT=unix:///var/run/containerd/containerd.sock

# Example for containerd
export CONTAINER_RUNTIME_ENDPOINT=unix:///Users/fabien/.docker/run/docker.sock
export IMAGE_SERVICE_ENDPOINT=unix:///Users/fabien/.docker/run/docker.sock
```
3. Docker ps
```
docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED        STATUS       PORTS                       NAMES
fd4141bc8034   kindest/node:v1.34.0   "/usr/local/bin/entr…"   2 months ago   Up 4 weeks                               my-kind-worker
aa629aa1691f   kindest/node:v1.34.0   "/usr/local/bin/entr…"   2 months ago   Up 4 weeks                               my-kind-worker2
fab136896ccd   kindest/node:v1.34.0   "/usr/local/bin/entr…"   2 months ago   Up 4 weeks   127.0.0.1:58040->6443/tcp   my-kind-control-plane
```
4. docker exec -it
```
docker exec -it my-kind-control-plane crictl ps -a

docker exec -it my-kind-control-plane crictl

crictl --help

docker exec -it my-kind-control-plane crictl ps
#OK get container id

docker exec -it my-kind-control-plane crictl exec -i -t 9a471f7847c71 sh
#OK get in the container

docker exec -it my-kind-control-plane crictl logs 9a471f7847c71
#OK get logs from the container id

#in the context of CKA ...
#if kubectl does not work
#if kube-apiserver or kubelet is down

ssh to node or control plane 

crictl info

crictl ps -a

crictl logs <container-id>

crictl inspect <container-id>
```
##

Key Functions and Modes of kubectl debug

The command has three main modes of operation:

1. Ephemeral Container (Recommended for Live Debugging)
This mode is used to inject a temporary, new container directly into an already running Pod. 
The ephemeral container shares the Network, PID, and IPC namespaces of the existing Pod, allowing you to debug network issues, check open ports, and inspect the filesystem of the main application container without restarting or affecting it.
When to Use: When the existing container's image (e.g., a security-hardened image) doesn't have tools like bash, netstat, or tcpdump.
Mechanism: The new container runs a tool-rich image (like busybox or nicolaka/netshoot) alongside the application container.
Example Command:
```
kubectl debug -it <pod-name> --image=nicolaka/netshoot --target=<app-container-name>
#FlagPurpose
#-it Interactive (connects to the new container's terminal).
#--image Specifies the debugging image to use (e.g., nicolaka/netshoot).
#--target(Optional, but recommended) Specifies the name of the container in the Pod whose processes the debug container should be able to inspect. Requires 
```
Process Namespace Sharing to be enabled on the Pod's configuration.

2. Copy and Edit Pod (Non-Live Debugging)
This mode creates a new copy of the original Pod with modifications, which is useful for debugging applications that are crashing immediately (i.e., in a CrashLoopBackOff state) or for isolating the issue from production traffic.
When to Use: When you need to prevent the application from crashing (e.g., by changing the command to sleep infinity) to debug it, or when you need to enable an feature like Process Namespace Sharing that cannot be added to a running Pod.
Mechanism: It takes the Pod's YAML specification, applies your changes, and creates a new, identically scheduled Pod.
Example Commands:
```
Copy and pause a crashing container:
kubectl debug <pod-name> --copy-to=debug-copy-pod --container=<crashing-container-name> --command=["sleep", "infinity"]
```
Copy and add a debug container with process sharing:Bashkubectl debug <pod-name> --copy-to=debug-copy-pod --image=busybox --share-processes

3. Node Debugging
This mode allows you to create a debug container that runs directly on the Node's host OS namespace, granting you powerful access to the node's filesystem and process tree.
When to Use: To troubleshoot node-level issues like volumes, networking with the CNI, or problems with the Kubelet or Container Runtime.
Mechanism: It creates a Pod that runs on the specified node and mounts the host's root filesystem (/host).
Example Command:
```
kubectl debug node/<node-name> -it --image=ubuntu
```
This command creates a new Pod on the target node where the root directory (/) of the debug container is mounted to the host's root directory under /host. This allows you to inspect system files, logs, and processes on the node itself.

-> make a summary !!! !!! !!!

##

More linux tips and tricks
```
ss -tunl
#to see the listening ports

systemctl cat kubelet

systemctl show kubelet --property=ActiveState,PID,Environment

# Forces systemd to re-read all unit files, essential after creating a new service
sudo systemctl daemon-reload

# Disable a service so it can never be started, even by dependencies
sudo systemctl mask <service-name>

# Re-enable the service
sudo systemctl unmask <service-name>

sysctl -a | grep net.ipv4
sysctl net.ipv4.ip_forward
sudo sysctl net.ipv4.ip_forward=1

# Test TCP connectivity to the API Server on port 6443
nc -v <api-server-ip> 6443

# Listen on a port for testing incoming firewall rules
nc -l -p 8080

#selinux/apparmor
sestatus
ls -Z /etc/kubernetes/manifests/
sudo aa-status

#find which process is using a specific file
sudo lsof | grep /path/to/mount

#find open sockets
sudo lsof -i :<port>
```

##

log locations to search api server crash !!!
```
/var/log/pods/
/var/log/containers/
crictl ps + crictl logs
docker ps + docker logs
/var/log/syslog 
journalctl -u kubelet

tail -F /var/log/pods/*
tail -F /var/log/containers/*
tail -f /var/log/syslog
```
##

We can get logs from all containers
```
kubectl -n management logs --all-containers deploy/collect-data > /root/logs.log
```
#OK

##

Get the labels from a ns. !!! !!!
```
kubectl get ns --show-labels
kubectl get all --show-labels
```
##

Network policies

```yaml
podSelector: {} #all pods
namespaceSelector: {} #all namespaces in the cluster

#Deny all
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: my-app-ns
spec:
  podSelector: {} # Selects ALL pods in 'my-app-ns'
  policyTypes:
    - Ingress
    - Egress
  ingress: [] # Deny all ingress
  egress: [] # Deny all egress (unless overridden by other policies)

#when combining ns selector and pod selector, the pod selector is within the ns selected
from:
  - namespaceSelector: # Selects the namespace
      matchLabels:
        name: other-namespace
    podSelector:     # Then, selects specific pods WITHIN that selected namespace
      matchLabels:
        app: other-app
```
A NetworkPolicy only applies to pods selected by its top-level spec.podSelector

The moment any NetworkPolicy selects a pod, all traffic to/from that pod is denied by default.

A NetworkPolicy is always namespace-scoped. Always specify the namespace in the metadata section.

##

When creating a ClusterRole that aggregates other roles, the annotation used is rbac.authorization.k8s.io/aggregate-to-...: "true"

##

The standard directory to store CA, API server and etcd certificates is /etc/kubernetes/pki.

##

Install containerd
```
sudo mkdir -p /usr/local/containerd
sudo tar Cxzvf /usr/local https://github.com/containerd/containerd/releases/download/v2.0.4/containerd-2.0.4-linux-amd64.tar.gz
sudo ln -sf /usr/local/bin/containerd /usr/local/sbin/containerd

cat <<'EOF_UNIT' | sudo tee /etc/systemd/system/containerd.service
> [Unit]
> Description=containerd container runtime
> Documentation=https://containerd.io
> After=network.target
> 
> [Service]
> ExecStart=/usr/local/bin/containerd
> Restart=always
> RestartSec=5
> RuntimeDirectory=containerd
> Delegate=yes
> KillMode=process
> OOMScoreAdjust=-999
> 
> [Install]
> WantedBy=multi-user.target
> EOF_UNIT
[Unit]
Description=containerd container runtime
Documentation=https://containerd.io
After=network.target

[Service]
ExecStart=/usr/local/bin/containerd
Restart=always
RestartSec=5
RuntimeDirectory=containerd
Delegate=yes
KillMode=process
OOMScoreAdjust=-999

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable --now containerd
```

##

Tips and tricks on kubernetes service investigation.

Endpoints: kubectl get endpoints <service-name>. If this is empty, your Service labels don't match your Pod labels.
kubectl get pods, svc -l xxx

##

Investigating kube-api access issues

CAN-I ?

1. netcat
```
nv -vz <api-server> 6443
```
2. check private
check if api endpoint is in private access

WHO AM I ?
```
kube config -> user -> certificate secret
kubectl get secret <secret-name> -o jsonpath='{.data.token}' | base64 -d
```
WHAT CAN I DO ?
```
kubectl auth can-i get pods -n my-ns

kubectl get clusterrolebindings -o wide | grep <user|group>
```
API SERVER INTERNAL HEALTH
```
curl -k https://<api-server>:6443/healthz
kubectl get --raw='\''/readyz?verbose'\'''

#check api server logs

kubectl get pods --v=8
```
3. Error codes.
```
000/Timeout : Network blocked
            FW rules, VPN down, LB issues.
401/Unauthorized : Bad credentials
            Expired token, invalided cert, wrong ctx.
403/Forbidden : RBAC issue
            Use exists but lacks role or clusterrole
429/Too many Requests : Throtling
            A script controller is spamming the api
500/503 : Server Error
            API server is crashing or etcd is down.
```
##

Tips and tricks working with multicontainers.


1. Lifecycle.
Native sidecars that start before the app but stay running.

2. Resource management.
Pod resource management = sum of req/limit of all containers.
```
kubectl top pod <pod-name> --containers
```
3. Communication.
Container A can reach B by localhost:port
You cannot have two containers using the same port in a pod.

4. EmptyDir.
```yaml
volumes:
- name: shared-data
  emptyDir: {}
```
5. Debug with -c.
```
kubectl logs <pod-name> -c <container-name>
kubectl exec -it <pod-name> -c <container-name> -- /bin/sh
kubectl logs <pod-name> --all-containers
````

##

The different ports.

|containerPort       |Pod/Deployment  |The port your application code is actually listening on (e.g., Node.js on 3000).|
|targetPort          |Service         |The port the Service sends traffic to. It must match the containerPort.|
|port                |Service         |The internal cluster port. Other pods will use this port to talk to your service.|
|nodePort            |Service         |A port opened on every Node's IP to allow external traffic (Range: 30000–32767).|

##

The Lifecycle of a Request (Visualizing the Strategy)
Imagine a user visiting your website. Here is how the ports line up in a professional "Production Strategy":

User Browser: Hits the Load Balancer on Port 443.

Load Balancer: Forwards traffic to the cluster. If using a NodePort service, it hits the Nodes on Port 30001 (an arbitrary high port).

Service: The request enters the Service on Port 80 (standardized internal port).

Endpoint: The Service looks at its Endpoints (the dynamic list of live Pod IPs).

Pod: The traffic finally reaches the Container on Port 8080 (where your actual code is running).

##

The list verb is to get multiple resources, while get is for a single one.

##

You need to ensure that no more than one pod of a specific deployment is scheduled on any signle node, and ensuring pods are spread accross different zones.
Using topologySpreadConstraints with whenUnsatisfiable: DoNotSchedule.

##

Regarding network policies when multiple selectors are listed as seperate items in a from array, they act as an OR condition.

##

You are creating a sidecar container in a pod. You want the sidecar to be able to see the container's processes. You need shareProcessNamespace: true in the pod spec.

##

When configuring a multi-container pod, you want one container to wait until a specific configuration file is generated by another container.
Using an init container to generate the file in a shared volume. Init containers must complete successfully before any app containers start, making init containers perfect for pre setup tasks.

##

LimitRange in a namespace is used to enforce default and min/max resource requests for individual containers.
ResourceQuota provide constraints that limit the total resource consumption (cpu, memory, ...) per namespace.


##

nodeSelector is the simplest way to constrain pods to nodes with specific labels, such as disk=ssd.
!!!

##

The endpoints object (or endpointslice object) lists the specific pod ips and ports that are ready to receive traffic.

##

Component,          Port,         Purpose
kube-apiserver,     6443,         The hub for all communication.
etcd,               2379,         The cluster database.
kubelet,            10250,        Agent on nodes; handles pod lifecycle.
NodePort Services,  30000–32767,  External traffic range.

##

links

https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster

https://kubernetes.io/docs/concepts/services-networking/network-policies/

https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

https://kubernetes.io/docs/concepts/storage/persistent-volumes/

##

vi tips and tricks

:set list
:set paste
G #go to bottom line
0 #go to beginning of line
:syntax on #enable color coding

##

other tips

The Trick: Use --force and --grace-period=0.

The Shortcut: alias kdel='kubectl delete pod --force --grace-period=0'

This is especially useful when fixing a "broken" Pod that is stuck in a Terminating state.


The Trick: If a question has 5 parts all in the same namespace, switch your default context temporarily: kubectl config set-context --current --namespace=my-namespace


Vim: echo "set ts=2 sw=2 et" > ~/.vimrc

Shell: export do="--dry-run=client -o yaml" (Usage: k run pod1 --image=nginx $do > p1.yaml)

##

```
/root/CKA/john.csr 

cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer # example
spec:
  # This is an encoded CSR. Change this to the base64-encoded contents of myuser.csr
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUtoR1htYWVVRnJtWUFiWkc4K0FsUE1wTkR2eVplSWtzbitxWnNCZlhXK3JRWlMvCklNa0h4b0svN3lhSGhyc2J5VmpuSEVzSnNEdXFUZ29kVStLcU5CaXl1V0pCSTB2d1lHMmh5WjRiRmt1bFFNTFcKeDdZT3B4V3Myc1l5Z0t6MkxDRFh6RWJLMXRjazBGWTErbWJXcm9VNUlBTVJ6SzRqZDI3MU5TR1ExTkM3dTNwMwppWEY3NE1vMjBUZmhjSWdNTUxtMWM4Z0JhTWJTa3Q0NmM0d3g0bzNCWWliandhVWhoRU9KbFRRdVZBTThvSTZsClVOMEdmY2xPdjhudXVZZU1tdkI0SEpmS0dhRjN4MEo4c3JRNXBybFArL1hYMzlOaXZORkxSdnBiNjAyY1gvRjEKazNtZWx1dzZXVWNIczBWRkM3bnhMb3QrNldaTVhyRGNWSGkyZGxrQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQU9JNXpqVnpoTW8xa0YwZlZaK0xIOGFzV2ZxS0JBUnBzNEpsVlZ5SnJGVm4wTC92MUJMYzJmCkQxcDUvdmRpeEVoeG9rR3ZKVnp2Z0ZBakdrRDJCQUMvcE55cUVsaXROQ3RYTXZkeFpGbkFqOHNGWk5LdmQrVDgKVkptczFWY3ZkWHhTek9ZZlJsZGFQTVFZZHJ5bEdKZmhkc2FoNUh4aDEwMm1BL3JIZDJZdlFuRmZ3N3hEdjZNVApMR0RZS3lvbjY0eFdlQjdFOWtQK1RJbW1Fb1VpZG9TcTY2T2RWZjM0N0QxSlpiRTVZMEY5QUl4SWp6aFpheGU4CkcwNm9TSjFPVU43RThjbG16M3dYcStudHlZL0tsWnVpRUUyRDJ6anVCZklOMzE2UDNKUHZBWGhEMmFUWUVXM2IKNmh4RFEvS2ZoeGErd0JQa29TdUtzSENTclNXK0RORFIKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF
```
```yaml
# webapp-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: webapp-ingress
  namespace: ingress-ns
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx # kubectl create ingress forget to do the ingress class TO CHECK ???
  rules:
  - host: kodekloud-ingress.app
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: webapp-svc
            port:
              number: 80
```

```
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup 172-17-1-13.default.pod
#each pod has an address in the form podIPseparatedwith-.default.pod
```

##

allow all on one port

---
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1
  policyTypes:
  - Ingress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
```
##

ResourceQouta issue
38s         Warning   FailedCreate        replicaset/backend-api-c5f6b4844    Error creating: pods "backend-api-c5f6b4844-mb5n6" is forbidden: exceeded quota: cpu-mem-quota, requested: requests.cpu=80m, used: requests.cpu=280m, limited: requests.cpu=300m

Understanding the Math
The error message gives you the exact numbers causing the "Forbidden" state:

Limited (Max Allowed): 300Mi

Used (Current): 256Mi

Requested (New Pod): 80Mi

Since 256Mi + 80Mi = 336Mi, which is greater than your 300Mi limit, Kubernetes blocks the creation of the Pod to prevent a quota violation.

or

Requested + Used > Limit

##

When do we need subPath ?
```yaml
volumeMounts:
        - mountPath: /etc/nginx/conf.d/default.conf
          name: nginx-conf-vol
          subPath: default.conf    
```
https://www.youtube.com/watch?v=R_qhriT8zWY

There are three main scenarios where subPath is essential:

Injecting a single configuration file: You want to put a config.php or settings.yaml into an existing directory (like /etc/nginx/) without deleting the other files already there.

Multiple containers using the same volume: You have a single Persistent Volume (PV) and you want Container A to use /data/logs/app1 and Container B to use /data/logs/app2.

Database paths: When using a volume for a database (like MySQL), you often want the data to sit in a subfolder of the volume to keep the root directory clean of "lost+found" files that some filesystems create.

Examples.
```yaml
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: config-volume
      mountPath: /etc/nginx/nginx.conf # The full destination path
      subPath: nginx.conf             # The key/file inside the volume
  volumes:
  - name: config-volume
    configMap:
      name: my-nginx-config
```
```yaml
spec:
  containers:
  - name: app-1
    volumeMounts:
    - name: shared-data
      mountPath: /var/lib/data
      subPath: app1-files # Mounts [VolumeRoot]/app1-files to /var/lib/data
  - name: app-2
    volumeMounts:
    - name: shared-data
      mountPath: /var/lib/data
      subPath: app2-files # Mounts [VolumeRoot]/app2-files to /var/lib/data
  volumes:
  - name: shared-data
    persistentVolumeClaim:
      claimName: my-pvc
```

##

A gateway listener has a hostname and an allowedroutes.

Here is an example.
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: prod-gateway
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    protocol: HTTP
    port: 80
    allowedRoutes:
      namespaces:
        from: Same # Only routes in the same namespace can attach
  - name: https
    protocol: HTTPS
    port: 443
    hostname: "api.example.com"
    tls:
      mode: Terminate # Terminate SSL at the gateway
      certificateRefs:
      - name: site-cert-secret
    allowedRoutes:
      namespaces:
        from: All # Any namespace can attach an HTTPRoute to this HTTPS listener
```

Protocol vs. Kind

The protocol in the listener (like HTTPS) dictates which kind of route can be used.

If protocol is HTTP/HTTPS, you attach HTTPRoutes.
If protocol is TCP, you attach TCPRoutes.

The "Handshake" (AllowedRoutes)

This is the most powerful part of the Gateway API. It allows a cluster admin to control delegation. Using the allowedRoutes field, the admin can say:
"Only the marketing team can attach routes to this listener."
"Only HTTPS routes are allowed; no plain text HTTP."

Hostname Isolation

You can define multiple listeners on the same port (e.g., 443) as long as they have different hostnames. This allows you to terminate SSL with different certificates for app1.com and app2.com on a single shared Load Balancer IP.

```yaml
# Create the Gateway
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: web-gateway
  namespace: nginx-gateway
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    port: 80
    protocol: HTTP
    allowedRoutes: #MISSING
      namespaces:  #MISSING
        from: All  #MISSING
```
This field defines the relationship between the Gateway (usually managed by Cluster Admins) and the Routes (managed by App Developers).
It has three main filters: namespaces, kinds, and selector.

1. Filtering by namespace.
```yaml
allowedRoutes:
  namespaces:
    from: Selector
    selector:
      matchLabels:
        exposed: "true"
```
2. Filtering by kind.
```yaml
allowedRoutes:
  kinds:
  - kind: HTTPRoute  # Only HTTP traffic allowed, no TCP/UDP
```
##

where to find documented annotation for ingress redirect ssl ?
for the exam ...
in the search of the kubernetes documentation

##

Simplest method to get the env from a CM
```yaml
    spec:
      containers:
      - image: kodekloud/webapp-color
        name: webapp-color-wl10
        envFrom:
        - configMapRef: 
            name: webapp-wl10-config-map
```
##

Tip on get event 
```
kubectl get event --field-selector involvedObject.name=<pod-name>
```
##

```yaml
#can we do shorter than ?

    env:
      - name: SECRET_USERNAME
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: username
      - name: SECRET_PASSWORD
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: password

#yes we can with envFrom (could also be used for CM)
    envFrom:
      - secretRef:
          name: mysecret

#envFrom is a streamlined way to inject multiple environment variables into a container at once
#envFrom tells the container to "import" all key-value pairs from an entire Secret or ConfigMap
```
##
```
crictl ps
CONTAINER           IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID              POD                                             NAMESPACE
a91a26f74cd93       a389e107f4ff1       22 seconds ago      Running             kube-scheduler            1                   cb9d7f8fabcb7       kube-scheduler-cluster2-controlplane            kube-system
2db71e154d0de       c2e17b8d0f4a3       22 seconds ago      Running             kube-apiserver            0                   1715fbb473c81       kube-apiserver-cluster2-controlplane            kube-system
ea44779525109       8cab3d2a8bd0f       22 seconds ago      Running             kube-controller-manager   1                   4f48d090ccc06       kube-controller-manager-cluster2-controlplane   kube-system
626009f1c8450       ead0a4a53df89       3 hours ago         Running             coredns                   0                   e8238b81f4aa9       coredns-7484cd47db-mr76f                        kube-system
6b00ea482c35c       6331715a2ae96       3 hours ago         Running             calico-kube-controllers   0                   464475361bc74       calico-kube-controllers-5745477d4d-djv7g        kube-system
8b043cc502ca4       ead0a4a53df89       3 hours ago         Running             coredns                   0                   ffe4aea0a3b9f       coredns-7484cd47db-nlpv5                        kube-system
4106f15ea937f       c9fe3bce8a6d8       3 hours ago         Running             kube-flannel              0                   0e600af90ba7b       canal-zklr9                                     kube-system
43901376ba258       feb26d4585d68       3 hours ago         Running             calico-node               0                   0e600af90ba7b       canal-zklr9                                     kube-system
05d201ef4b737       040f9f8aac8cd       3 hours ago         Running             kube-proxy                0                   b068a33d17064       kube-proxy-bk6tr                                kube-system
f44c170134dad       a9e7e6b294baf       3 hours ago         Running             etcd                      0                   a2310dc49e5ed       etcd-cluster2-controlplane                      kube-system
```
##
```
ss -tlnp | grep 6443
LISTEN 0      4096                *:6443             *:*    users:(("kube-apiserver",pid=88077,fd=3)) 
#api server should be running
```
##

The connection to the server cluster2-controlplane:6443 was refused - did you specify the right host or port?

1. Check if api server is running.
```
crictl ps 
docker ps
```
If not running chekck the api manifest.
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml
#check typo in manifests
```
Check kubelet logs.
```
systemctl status kubelet
journalctl -u kubelet -f
```
2. Verify kubeconfig.

- check .kube/config
  - check the permissions
  - check the content
  - check the ports number
  - check the certificates
  - check server address
```
kubeadm certs check-expiration
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout | grep Not
```
3. Check api server logs.
```
crictl logs <container-id>
```
4. Check pot/process listeners.
```
ss -nltp | grep 6443
nc -vz <api-server-ip> 6443
```
5. Where to find api server ip if down.
```
grep server /etc/kubernetes/kubelet.conf
grep -E "advertise-address|bind-address" /etc/kubernetes/manifests/kube-apiserver.yaml
grep "server:" /etc/kubernetes/admin.conf
grep "server:" ~/.kube/config
grep server /etc/kubernetes/*.conf
```

##
```
kubectl -n spectra-1267 get pods --show-labels -l=mode=exam,type=external
NAME     READY   STATUS    RESTARTS   AGE     LABELS
pod-21   1/1     Running   0          3m44s   env=prod,mode=exam,type=external
pod-23   1/1     Running   0          3m44s   env=dev,mode=exam,type=external
```
##

how to create an external service pointing to an external IP outside of the cluster
create an ep manually

##

ensure that service has an ep 
if not re-create it with expose
#OK

##

when modifying cm coredns
```
kubectl rollout restart deploy -n kube-system coredns
```
##

1. Install the Flannel CNI
Download the Flannel manifest file:
```
wget https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
Edit the Flannel manifest to set the CIDR to 172.17.0.0/16:
```
vi kube-flannel.yml
```
Below is the structure of the CoreDNS configuration:
```yaml
apiVersion: v1
data:
  cni-conf.json: |
    {
      "name": "cbr0",
      "cniVersion": "0.3.1",
      "plugins": [
        {
          "type": "flannel",
          "delegate": {
            "hairpinMode": true,
            "isDefaultGateway": true
          }
        },
        {
          "type": "portmap",
          "capabilities": {
            "portMappings": true
          }
        }
      ]
    }
  net-conf.json: |
    {
      "Network": "172.17.0.0/16",   #Edited
      "EnableNFTables": false,
      "Backend": {
        "Type": "vxlan"
      }
    }
kind: ConfigMap
metadata:
  labels:
    app: flannel
    k8s-app: flannel
    tier: node
  name: kube-flannel-cfg
  namespace: kube-flannel
```
```
#Apply the manifest by running the following command:

kubectl apply -f kube-flannel.yml
```
2. Test the pod-to-pod communication.
```
#Run an nginx image:
kubectl run web-app --image nginx

#Retrieve the IP address of the pod:
kubectl get pod web-app -o jsonpath='{.status.podIP}'

#Test the connection:
kubectl run test --rm -it -n kube-public --image=jrecord/nettools --restart=Never -- curl <IP>

#TO FIND IN DOCUMENTATION !!!
otherwise below
```
##

Install Flannel.
```
wget https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
#OK

#the cluster cidr can be found in kube-controller-manager

grep cluster-cidr *
kube-controller-manager.yaml:    - --cluster-cidr=172.17.0.0/16

vi kube-flannel.yml
#OK modified net-conf.json

kubectl apply -f kube-flannel.yml
#OK
```
##

how to test an ingress

```
NAME                       CLASS    HOSTS   ADDRESS   PORTS   AGE
nginx-ingress-cka04-svcn   <none>   *                 80      47s

get the service 

kubectl get svc nginx-service-cka04-svcn (of the ingress)
NAME                       TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
nginx-service-cka04-svcn   ClusterIP   10.43.97.177   <none>        80/TCP    59m

and curl on it 
curl -I 10.43.97.177
HTTP/1.1 200 OK
...
#OK
```

##

can sort by in columns
```
kubectl -n spectra-1267 get pods -o custom-columns='POD_NAME:.metadata.name,IP_ADDR:.status.podIP' --sort-by=.status.podIP
```
##

How to solve OOMKilled pods 
Container is killed because the it exceeded the memory limit.
To fix increase the memory limit.
The node could also be out of memory.

##

external services

from endpointslices documentation
```
#
cluster3-controlplane  ~ ➜ kubectl  apply -f - <<EOF
apiVersion: discovery.k8s.io/v1
kind: EndpointSlice
metadata:
  name: external-webserver-cka03-svcn
  namespace: kube-public
  labels:
    kubernetes.io/service-name: external-webserver-cka03-svcn
addressType: IPv4
ports:
  - protocol: TCP
    port: 9999
endpoints:
  - addresses:
      - 192.168.222.128   # IP of student node
EOF
```
##

How to curl in kubernetes ?

1. Curl a pod.
- from another pod 
```
curl <pod-ip>:<port> #container port
```
- via port-forward

```
kubectl port-forward pod/<pod-name> <target-port>:<port>
```
2. Curl a service.
- by name
```
curl <service-name>.default.svc.cluster.local
```

- a clusterIP type service
```
curl <cluster-ip>:<port>
```
3. Curl an ingress.

- with a header
```
curl -H "Host: app.example.com" http://<ingress-controller-ip>
```
- with a resolve (etc hosts like)
```
curl --resolve app.example.com:<ingress-ip> http://app.example.com
```
4. Gateway ip.
```
export GW_IP=$(kubectl get gtw <GATEWAY_NAME> -o jsonpath='{.status.addresses[0].value}')
curl -H "Host: my-app.com" http://$GW_IP/v1/api
```
5. curlimages/curl
```
kubectl run curl-test --image=curlimages/curl --rm -it -- sh
# Une fois dans le shell, vous pouvez tester vos services et IPs
```

##

Most needed images for test. !!! !!! !!!

1. The best.
```
nicolaka/netshoot
```
2. The dns.
```
infoblox/dnstools
```
3. Busybox.
```
busybox
```
4. Alpine.
```
alpine #package manager: apk
```
5. Curl.
```
curlimages/curl
```
6. Clients.
```
postgres
mysql
redis
minio/mc #s3 compatible client
```
7. Cloud providers.
```
amazon/aws-cli
google/cloud-sdk
```
8. Security and scan.
```
praqma/network-multitool #networkpolicy
owasp/zap2docker-stable #pen tests
aquasec/trivy
```
9. Performance testing.
```
williamyeh/wrk
polandj/stress-ng
codesenberg/bombardier
```
10. Ephemeral containers.
```
kubectl debug -it <pod-name> --image=nicolaka/netshoot --target=<container-name>
```
11. Others.
```
alpine/k8s:1.XX #kubectl
telepresence/telepresence #IDE
sysdig/sysdig #what did my process before crashing ?
```

##

Curl options for debug.

1. Visibility.
```
-v #verbose
-i #include cleaner response
-s #silent
```
2. Network.
```
--resolver 
#for ingress testing
#etc hosts like
#curl -v --resolve app.example.com:443:10.0.0.5 https://app.example.com
-H 
#header
#curl -H "Host: api.internal.com" http://<load-balancer-ip>
```
3. TLS/SSL.
```
-k or --insecure
--cacert #specify a custop CA
--cert and --key
```
4. Performance.
```
curl -so /dev/null -w "\
  DNS Lookup:  %{time_namelookup}s\n\
  TCP Connect: %{time_connect}s\n\
  App Connect: %{time_appconnect}s\n\
  TTFB:        %{time_starttransfer}s\n\
  Total:       %{time_total}s\n" \
  http://nginx-resolver-service.default.svc.cluster.local
#OK

curl -so /dev/null -w "\
  DNS Lookup:  %{time_namelookup}s\n\
  TCP Connect: %{time_connect}s\n\
  App Connect: %{time_appconnect}s\n\
  TTFB:        %{time_starttransfer}s\n\
  Total:       %{time_total}s\n" \
  http://nginx-resolver-service
#OK

curl -so /dev/null -w "\
  DNS Lookup:  %{time_namelookup}s\n\
  TCP Connect: %{time_connect}s\n\
  App Connect: %{time_appconnect}s\n\
  TTFB:        %{time_starttransfer}s\n\
  Total:       %{time_total}s\n" \
  http://www.google.com
#OK

alias kcurl='curl -so /dev/null -w "\n\033[1;36m»» Debugging: %{url_effective}\033[0m\n\n\
\033[1;33m[Connection Times]\033[0m\n\
DNS Lookup      :  %{time_namelookup}s\n\
TCP Connect     :  %{time_connect}s\n\
TLS Handshake   :  %{time_appconnect}s\n\n\
\033[1;32m[Performance]\033[0m\n\
Time to 1st Byte:  %{time_starttransfer}s\n\
Total Time      :  %{time_total}s\n\n\
\033[1;35m[Status]\033[0m\n\
HTTP Code       :  %{http_code}\n\
Size Download   :  %{size_download} bytes\n\n"'
#OK 
#color codes to debug
```
##

Installing Flannel and Calico

Flannel (no netpol) and Calico (netpol ready)

1. Flannel
```
# download
curl -O https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

# Example: Changing 10.244.0.0/16 to 10.10.0.0/16
sed -i 's/10.244.0.0\/16/10.10.0.0\/16/g' kube-flannel.yml

kubectl apply -f kube-flannel.yml
```
2. Calico
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/tigera-operator.yaml
#install the operator

curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/custom-resources.yaml
#download the crds
```
```yaml
# Inside custom-resources.yaml
spec:
  calicoNetwork:
    ipPools:
    - blockSize: 26
      cidr: 10.10.0.0/16 # <-- Change this value here
      encapsulation: VXLANCrossSubnet

kubectl create -f custom-resources.yaml
#apply
```
3. Places to find CIDR
```
#kube-proxy
kubectl describe pod -n kube-system kube-proxy-xxxx | grep cluster-cidr

#kube-controller-manager
kubectl describe pod -n kube-system kube-controller-manager-control-plane | grep cluster-cidr
```
4. Others
```
# check kubelet logs
journalctl -u kubelet -f

#check if another CNI exists
ls /etc/cni/net.d/
#10-flannel.conflist or 10-calico.conflist

#check the CNI pods
kubectl -n kube-system get pods
```

##

What to do when the service do not have an endpoint specified ?

- Unmatch between pods labels and service selector.
- External service, service has no selector. The ep has to be created manually.

1. Fix the svc selector.
- check pod labels
```
kubectl get pods --show-labels
```
- check service selector
```
kubectl describe svc <service-name>
```
- fix the service selector
```
kubectl edit svc <service-name>
```
2. Check if pod is ready.

Kubernetes do not add ep to the service if pod is not ready.

3. External service.

If you are pointing to something outside the cluster, the service do not have a selector.
You muste create the ep manually.

From documentation https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/
```yaml
apiVersion: discovery.k8s.io/v1
kind: EndpointSlice
metadata:
  name: example-abc # -> use same as service name
  labels:
    kubernetes.io/service-name: example
addressType: IPv4
ports:
  - name: http
    protocol: TCP
    port: 80 # -> use external port 
endpoints:
  - addresses:
      - "10.1.2.3" # -> use external IP


apiVersion: v1
kind: Service
metadata:
  name: example-abc # -> same as endpoint name
spec:
  ports:
    - port: 5432
      targetPort: 5432     
```

Summary Checklist

1. Check Labels: Do Pod labels = Service selector ? !!! !!! !!!
2. Check Readiness: Are Pods 1/1 Ready?
3. Check Ports: Does the Service targetPort match the containerPort of the Pod?
4. Named Ports: If using names (e.g., port: http), ensure the name is defined identically in the Pod spec.

##

What can we edit and not in a deploy, pod, svc, ... ?

It depend on the controller managing the object and whether the field is immutable.

Deployments / StatefulSets / DaemonSets: Almost everything can be edited.
Services: You can edit ports, selectors, and types.
Pods: Pods are highly immutable.
ConfigMap / Secrets: Everything.

Use explain to know what is immutable or editable.

Common Immutable Fields:

PersistentVolumeClaims: storageClassName and accessModes.
Services: spec.clusterIP.
Jobs: spec.selector (after creation).

Edit alternatives.
- Replace.
```
# Export the current pod to YAML, change it, and replace it
kubectl get pod <name> -o yaml > pod.yaml
# Edit pod.yaml manually, then:
kubectl replace --force -f pod.yaml
```
- Set.
```
kubectl set image deployment/my-deploy nginx=nginx:1.19.1
```
Tip to use VSC to edit kubernetes objects.
```
export KUBE_EDITOR="code --wait"
```
##

how to come to the begining and end of the line on mac book terminal ?

##

```
kubectl  apply -f - <<EOF
apiVersion: discovery.k8s.io/v1
kind: EndpointSlice
metadata:
  name: external-webserver-cka03-svcn
  namespace: kube-public
  labels:
    kubernetes.io/service-name: external-webserver-cka03-svcn
addressType: IPv4
ports:
  - protocol: TCP
    port: 9999
endpoints:
  - addresses:
      - 192.168.222.128   # IP of student node
EOF
```

##

error: no objects passed to apply
no objects

##

how to know which ingressclass to use when not specified ?

##

Normal  WaitForFirstConsumer  1s (x10 over 2m11s)  persistentvolume-controller  waiting for first consumer to be created before binding

Immediate (Default): The PVC is bound to a PV as soon as the claim is created.
WaitForFirstConsumer: The PVC stays in a Pending state until a Pod (the "consumer") that uses this PVC is created and scheduled.

##

kubectl create vs kubectl apply

1. create 

- creates a new resource
- in error if already exists
- quick

2. apply

- creates or update
- creates it if do not exist
- updates if already exist


##

Just another debug set of commands.

📌 Pod Health (Start Here)

1. kubectl describe pod: Your main detective tool. Shows detailed state and events (scroll to the bottom for errors!).

2. kubectl top pod: Checks current CPU & RAM usage to spot resource bottlenecks.

3. kubectl get events --sort-by...: A chronological timeline of what happened recently in the cluster.

4. kubectl get pods -o wide: The big picture view. See the Pod IP and exactly which Node it's running on.

📌 Logs & Crashes (Why did it break?)

5. kubectl logs <pod>: View the current application logs.

6. kubectl logs -p <pod>: Life-saver for crash loops. Shows logs from the previous time it died.

7. kubectl logs ... -c <container> --previous: Same as above, but targeting a specific container in multi-container pods.

8. stern / kubetail: Tools to live-tail logs from many pods simultaneously.

📌 Get Inside

9. kubectl exec -it ... sh: Opens an interactive terminal inside the container to explore files or test connectivity.

📌 Configs & Connections

10. kubectl get deployment -o yaml: Dumps the full, raw configuration to check for setup errors.

11. kubectl get endpoints: Verifies your Service is actually sending traffic to healthy pods.

📌 Infrastructure & Access

12. kubectl get nodes | grep NotReady: The quickest way to find broken underlying servers.

13. kubectl auth can-i: Getting permission denied? This checks if you are allowed to perform a specific action.

##

what makes (which) control plane components to run in kube-system as pods or in systemd as processes ?

1. Kubeadm - Static pods
- kubelet is running in systemd, it will check in /etc/kubernetes/manifests and apply
- kubectl -n kube-system get pods
- in /etc/kubernetes/manifests
- crictl logs or kubectl logs
- edit the manifests files

2. The hard way - systemd - 
- systemctl status <service>
- in /etc/systemd/system
- journalctl -u <service>
- edit the services and systemctl daemon-reload

3. Always on systemd (because it is needed)
- containerd or docker 
- kubelet

4. Tip
If a control plane component is not found in /etc/kubernetes/manifests it is a systemd service: systemctl list-units-files

##
```
kubectl top pods -A --sort-by=memory
```
##

```
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer 
spec:
  # This is an encoded CSR. Change this to the base64-encoded contents of myuser.csr
  request: LSXXXXXXXXXXXXXXXXXXtCg==
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF

kubectl get csr john-developer -o jsonpath='{.status.certificate}'| base64 -d > john.crt

kubectl config set-credentials john --client-key=/root/CKA/john.key --client-certificate=john.crt --embed-certs=true
```
##

where to find the pod IP name ? 

##

```
# To see the flags of the API server
kubectl get --raw "/flagz"

# To see the status of the API server
kubectl get --raw "/statusz"

# Example JSON request for v1.35
curl -H "Accept: application/json" https://<api-server-ip>/statusz

```

Both K8sGPT and HolmesGPT are leaders in the "AI SRE" movement, but they approach troubleshooting from different angles. If K8sGPT is an expert inspector, HolmesGPT is an autonomous investigator.

##

Which resource is strictly required to allow a Gateway in 'Namespace A' to reference an HTTPRoute in 'Namespace B' safely?

ReferenceGrant

This resource provides a formal handshake that allows cross-namespace references, ensuring the owner of the target namespace explicitly permits the access.

```yaml
apiVersion: gateway.networking.k8s.io/v1beta1
kind: ReferenceGrant
metadata:
  name: allow-gateway-to-secrets
  namespace: certificate-mgr        # Where the protected resource lives
spec:
  from:                             # Who is asking?
    - group: gateway.networking.k8s.io
      kind: Gateway
      namespace: infra
  to:                               # What are they allowed to see?
    - group: ""
      kind: Secret
```

Key Components:

from: Defines the source of the reference (The "Client").

to: Defines the target of the reference (The "Resource").

Important Rules to Remember

Directional: The ReferenceGrant must reside in the same namespace as the resource being referred to (the target).

No "Self-Grant": You don't need a ReferenceGrant for resources within the same namespace.

Strictness: If a ReferenceGrant is deleted, the connection is severed, and the Gateway or Route will stop functioning for that 
cross-namespace link.

##

A Cluster Operator wants to apply a specific load-balancing algorithm (e.g., Least Connections) across all Gateways of a certain type. Where is the most appropriate place to define this implementation-specific configuration?

The GatewayClass uses parametersRef to point to vendor-specific configurations that apply to all Gateways using that class.

##

Policy Attachment is a core design pattern in the Gateway API intended to keep the main resources clean while allowing for extensible features.

##

One HttpRoute can be attached to many Gateways.
The 'parentRefs' field in a Route is a list, allowing one set of routing rules to be reused across different Gateways.

##

La sécurité TLS d'une requête.

client  -->  downstream (flux descendant) --> gateway (nginx or kong) -->  upstream (flux montant) -->  service

TLS mode:
- passthrough (la gateway ne touche pas au chiffrement)
- terminate (fin de la requête chiffrée, généralement en clair ensuite)

##

https://github.com/kubernetes
https://github.com/kubernetes/enhancements

##

Ensure no CR in secrets
```bash
# -n ensures no trailing newline/carriage return is added
kubectl create secret generic my-secret --from-literal=password=$(echo -n 'my-password')

# Removes all \r characters and creates the secret
tr -d '\r' < secret-file.txt > clean-file.txt
kubectl create secret generic my-file-secret --from-file=config=clean-file.txt

# Use -n with echo and --decode-free pipes
echo -n "my-password" | base64

# a better alternative for most systems
kubectl create secret generic my-secret --from-literal=password=$(printf %s 'my-password') 

kubectl get secret my-secret -o jsonpath="{.data.password}" | base64 --decode | cat -A
```
##

Review this one ...

kind: StorageClass # Le menu
apiVersion: storage.k8s.io/v1
metadata:
  name: orange-stc-cka07-str
provisioner: kubernetes.io/no-provisioner #no automatic disk creation, the admin must create the volume
volumeBindingMode: WaitForFirstConsumer #this storage will be attached only when a first pod will need it
                                        #ensure the storage is on the same node as the pod
---
apiVersion: v1
kind: PersistentVolume # Le disque
metadata:
  name: orange-pv-cka07-str
spec:
  capacity:
    storage: 150Mi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: orange-stc-cka07-str
  local:
    path: /opt/orange-data-cka07-str #this is local to a node, so it must start on a specific node -> node affinity
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - cluster1-controlplane
---
kind: PersistentVolumeClaim # La commande
apiVersion: v1
metadata:
  name: orange-pvc-cka07-str
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: orange-stc-cka07-str
  volumeName: orange-pv-cka07-str
  resources:
    requests:
      storage: 128Mi

##

When creating a storage solution what are the common solutions ?

pv + pvc (static provisioning) : no provisionner or empty storageClassName
pv + storageclass (dynamic provisioning) : the provisioner will specify the driver or the plugin (aws ebs, pd gce, local, ...)
pv + pvc + storageclass :
- you have a pv and a pvc that are linked but you want a sc for organization purpose
- local persistent volume
- importing existing disk

pv + pvc + pod
pv + storageclass + pod
pv + pvc + storageclass + pod

For storage there are twos scenarios
SC + PV + PVC
SC + PVC (the PV is created by the SC)
There is no way to get the PV or PVC created by the pod.

##

To be trained on external services with endpointslice be able to build a web server from a python everywhere if needed.

Feature	    Python 2 (Legacy)	    Python 3 (Current)
Module Name	SimpleHTTPServer	    http.server

Common Error	None	ModuleNotFoundError
Default Port	8000	8000

```
#with python2

python -m SimpleHTTPServer 8000

#with python3

python -m http.server 8000
python3 -m http.server 8000

#there is a security risk because you are serving files localy

#you can serve a directory

python3 -m http.server 8000 -d /var/log/nginx
```
##

This volume must be created on cluster1-node01
-> 
```yaml
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - cluster1-node01
```
!!! !!! !!!

##

How to verify the certificates are valid ?

- PKI certificates and requirements (dcoumentation)
- Certificate Management With Kubeadm (documentation)
```
kubeadm certs check-expiration
```
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep -E "cert|key|ca"

openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout

The logs will usually scream x509: certificate signed by unknown authority or no such file or directory if a path in the manifest is wrong.

##

Verify a CNI plugin is OK.

Deploy two simple Pods (like nginx) on different nodes.

Exec into one Pod: kubectl exec -it <pod-a> -- sh.

Try to ping the IP of the second Pod: ping <pod-b-ip>.

##

The "Big Picture" Flow

When you run kubectl apply -f deployment.yaml, look at the "Chain of Command":
1. Deployment Controller sees the new YAML $\rightarrow$ Creates a ReplicaSet.
2. ReplicaSet Controller sees the new ReplicaSet $\rightarrow$ Creates Pod objects (pending).
3. The Scheduler (a special kind of controller) $\rightarrow$ Assigns the Pods to a Node.
4. Kubelet (the agent on the node) $\rightarrow$ Sees the assignment $\rightarrow$ Talks to Container Runtime (Docker/CRI-O) to pull the image and run it.
5. Endpoints Controller $\rightarrow$ Notices the new Pod IPs $\rightarrow$ Updates the EndpointSlice.

If you understand these loops, troubleshooting becomes logical. If a Pod won't start:
Is the Scheduler failing to find a node?
Is the ReplicaSet failing to trigger?
Is the Kubelet failing to talk to the runtime?

##

1. The "Single Source of Truth": etcd
Everything in Kubernetes is just a row in a database. When you run kubectl apply, you aren't "running a command"—you are updating a record in etcd.
Why this is the key: The API Server is essentially a gatekeeper for etcd. Every other component (Kubelet, Scheduler, Controllers) is just a "watcher" that subscribes to updates from this database.
Understanding the Internal: Kubernetes doesn't "push" commands to nodes. Instead, the Node (Kubelet) "watches" the API Server and says, "Oh, I see my name next to a new Pod in the database. I better start it."

2. Labels and Selectors: The "Glue"
In traditional IT, we used IP addresses or specific hostnames to connect things. In Kubernetes, nothing is hard-coded.
Loose Coupling: A Service finds a Pod not by its name, but by its Labels.
The Key: This allows Kubernetes to be "self-healing." If a Pod dies and a new one is created with a different IP, the Service doesn't care—it just looks for the Label again and finds the new IP instantly.

3. The Kubelet: The "Worker Bee
"While the Control Plane (the "Masters") makes the big decisions, the Kubelet is the most important component on the worker nodes.The "PodSpec" contract: The Kubelet receives a PodSpec (a YAML description of a Pod). Its only job is to ensure the containers described in that spec are running and healthy.
The Key Concept: The Kubelet doesn't care about "Deployments" or "StatefulSets." It only sees "Pods." It bridges the gap between the high-level world of Kubernetes APIs and the low-level world of Linux (Namespaces, Cgroups, and Docker/Containerd).

4. The "Common Language": The API Object Model
To truly "get" Kubernetes, you have to realize that everything is an Object.
A Node is an object.
A Secret is an object.
An Event is an object.
A Pod is an object.
They all follow the same structure:
Metadata: Name, Namespace, Labels.Spec: What you want (Desired State).
Status: What is actually happening (Current State).

Summary: The "Mental Map"
To master Kubernetes for the CKA or real-world architecture, look at it like this:
LevelComponentKey Concept
Brain         etcd        The only place where state is stored.
Voice         API Server  The only way to talk to the brain.
Muscle        Kubelet     The bridge between YAML and real containers.
Logic         Controllers The "Reconciliation Loop" (Desired vs. Current).
Connectivity  Labels      The dynamic "glue" that ignores IPs and names.

The "Aha!" Moment
The biggest secret to understanding Kubernetes is realizing it is asynchronous. When you run a command, it doesn't happen instantly. You are just changing the "Desired State" in the database, and then waiting for the "Nervous System" (Controllers) to notice and react.

###########

Investigations on kubectl unavailable.

VERIFY ENTRY POINT

1. Is kube-apiserver running ?

crictl ps | grep kube-apiserver

2. Check port binding.

sudo netstat -tulnp | grep 6443

VERIFY THE CHAIN

1. Kubelet

sudo systemctl status kubelet

sudo journalctl -u kubelet -f

Common Issue: Look for "Certificate expired" or "CNI plugin not initialized."

2. etcd

crictl ps | grep etcd

#check etcd health
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt \
  --key=/etc/kubernetes/pki/etcd/healthcheck-client.key \
  endpoint health

3. Manifests

Check /etc/kubernetes/manifests

3.1. Watch

watch -n 1 sudo crictl ps -a

3.2. Search for manifests errors in system logs

sudo journalctl -u kubelet | grep -i "manifest"

3.3. crictl logs

sudo crictl ps -a | grep kube-apiserver | head -n 1
# Replace <ID> with the result from the command above
sudo crictl logs <ID>

3.4. re launch

# 1. Move it out to your home folder
sudo mv /etc/kubernetes/manifests/kube-apiserver.yaml ~/

# 2. Wait 30 seconds (ensure 'crictl ps' shows it is gone)

# 3. Move it back
sudo mv ~/kube-apiserver.yaml /etc/kubernetes/manifests/

3.5. yaml linter

python3 -c 'import yaml, sys; yaml.safe_load(sys.stdin)' < /etc/kubernetes/manifests/kube-apiserver.yaml


3.6. kubeadm init dry-run

kubeadm init phase control-plane all --dry-run
#This command will detect the file is missing and generate a brand-new one based on the configuration stored in the kubeadm-config ConfigMap in your cluster.

3.7. compare with a secondary control plane node

diff /etc/kubernetes/manifests/*.yaml /tmp/kubernetes/manifests/*.yaml

4. System

sudo journalctl -k



