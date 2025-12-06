# CKA

## Book the exam
- Book the exam: https://trainingportal.linuxfoundation.org/learn/dashboard/
  - -> Resume
  - There is availability for friday morning for instance ... in two weeks ...
- Take the killer.sh (2 times)  
  https://trainingportal.linuxfoundation.org/learn/course/certified-kubernetes-administrator-cka/exam/exam
- Use code `30KK` to get 30% off.

---

## TODO
- Configure KodeKloud on discord: https://discord.gg/VAfhT6ZR9E
- Take the official simulation: 2 mock exams (Killer.sh)
- ID card needed for the exam (permis de conduire recommended)
- Must run exam on Chrome (Safari/Arc issues with mic)
- Prefer exam on a Tuesday 10:00–12:00

---

## LINKS
- Authorized documentation for the exam:
  - https://kubernetes.io/docs
  - https://kubernetes.io/blog/
  - https://helm.sh/docs
  - https://gateway-api.sigs.k8s.io/
- Useful resources:
  - https://killercoda.com/
  - https://notes.kodekloud.com/docs/CKA-Certification-Course-Certified-Kubernetes-Administrator/Introduction/Course-Introduction
  - https://raghu.sh/new-cka-exam-tips-tricks-ft-gateway-api-cni-cri-csi-helm-kustomize-ab5bb621b914

---

## MY LINKS / Tools
- Kodekloud notes, KillerCoda, multipass troubleshooting, KodeKloud course repo:
  - https://github.com/kodekloudhub/certified-kubernetes-administrator-course

---

## QUESTIONS
- Only one monitor accepted?
  - Replicate the monitors.
  - Clap the MacBook and use external keyboard, mouse, camera, and mic.
  - https://github.com/waydabber/BetterDisplay
- Where find kustomize documentation?
  - Use sig kubernetes GitHub kustomize docs?
  - How to get same as `kubectl explain kustomization`?

---

## Quick exam tips / common kubectl commands
- Create resources dry-run:
  - `kubectl run redis --image redis123 --dry-run=client -o yaml > redis.yaml`
  - `kubectl run nginx --image=nginx --dry-run=client -o yaml`
  - `kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`
  - `kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml`
- Expose:
  - `kubectl expose deploy nginx --port=80`
- Edit / Scale / Set image:
  - `kubectl edit deploy nginx`
  - `kubectl scale deploy nginx --replicas=5`
  - `kubectl set image deploy nginx nginx=nginx:1.21`
- Create service from pod (dry-run):
  - `kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`
  - `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`
- Imperative example:
  - `kubectl run httpd --image=httpd:alpine --port=80 --expose` (creates both service & pod)
- Labels & selectors:
  - `kubectl get pods --selector app=app1`

---

## Kubernetes core components (notes)
- Controller manager: controllers, kube-controller-manager (kube-system)
- Scheduler: kube-scheduler (makes scheduling decisions)
- Kubelet: runs on each node, manages pods/containers
- Kube-proxy: manages network connectivity (iptables or ipvs)

---

## Objects: Pods / ReplicaSet / Deployments
- Pod fields: kind, apiVersion, metadata, spec
- ReplicaSet: selector is mandatory
  - spec: template, replicas, selector
- Deployments:
  - `kubectl create deployment my-dep --image=nginx --replicas=3`
  - Strategies: Recreate, RollingUpdate (default)
  - Rollout: `kubectl rollout status deploy/xxx`, `kubectl rollout history deploy/xxx`, `kubectl rollout undo deploy/xxx`

---

## Services
- NodePort:
  - nodePort (30000-32767), port (clusterIP), targetPort (container)
- ClusterIP:
  - port, targetPort
- LoadBalancer:
  - nodePort, external LB port, targetPort

---

## Scheduling: Node selectors / Affinity / Taints & Tolerations
- NodeSelector example:
  - Node label: `kubectl label nodes node-1 size=Large`
  - Pod spec:
    ```
    spec:
      nodeSelector:
        size: Large
    ```
- NodeAffinity example (required -> scheduling only new pods):
  ```
  spec:
    nodeAffinity:
      required...
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values:
            - Large
  ```
- Taints/Tolerations:
  - Taints: NoSchedule, PreferNoSchedule, NoExecute (evict running pods without tolerance)

---

## Resources and Limits
- Best practice: set requests, optional limits
- LimitRange: default limits/requests per namespace
- ResourceQuota: hard limits in a namespace

---

## DaemonSets / Static Pods
- DaemonSet: run pod on each node, uses scheduler
- Static pod:
  - Managed by kubelet directly via files under `/etc/kubernetes/manifests`
  - Useful for control-plane static pods

Example to create static pod manifest:
```
kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```

---

## Priority Classes
- Range: 1_000_000_000 to -2_000_000_000
- Example: kube-system uses 2_000_000_000
- preemptionPolicy: PreemptLowerPriority

---

## Multiple schedulers & Scheduler configuration
- Scheduler config can have profiles and plugins (filtering, scoring, binding)
- Run scheduler as a pod with `--config=/etc/kubernetes/xxx.yaml`
- In pod spec, `schedulerName: xxx` to use a custom scheduler

---

## Admission controllers
- Examples: AlwaysPullImages, DefaultStorageClass, EventRateLimit, NamespaceLifecycle...
- Check enabled plugins:
  - `kube-apiserver -h | grep enable-admission-plugins`
  - For static manifests: `grep enable-admission-plugins /etc/kubernetes/manifests/kube-apiserver.yaml`

---

## Monitoring & Logging
- Metrics-server for kubectl top (external)
  - Install: `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`
- Kubelet uses cAdvisor
- Logs:
  - `kubectl logs -f <pod>`
  - `kubectl logs -f <pod> -c <container>`

---

## TLS & Certificates (openssl basics)
- CA:
  ```
  openssl genrsa -out ca.key 2048
  openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
  openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
  ```
- Client cert:
  ```
  openssl genrsa -out admin.key 2048
  openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
  openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
  ```
- Server cert (with SANs via config):
  - Use openssl.cnf alt_names for DNS entries
- View cert:
  - `openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`

---

## CSRs via Kubernetes API
- Example CSR manifest (base64-encoded CSR):
  ```
  apiVersion: certificates.k8s.io/v1
  kind: CertificateSigningRequest
  metadata:
    name: akshay
  spec:
    groups:
    - system:authenticated
    request: <base64 CSR>
    signerName: kubernetes.io/kube-apiserver-client
    usages:
    - client auth
  ```

---

## Kubeconfig & contexts
- View: `kubectl config view`
- Use context: `kubectl config use-context prod-user@production`
- Set KUBECONFIG env: `export KUBECONFIG=/root/my-kube-config`

---

## Authorization & RBAC
- `kubectl auth can-i <verb> <resource> [--as=USER]`
- Create Role / RoleBinding examples:
  - `kubectl create role developer --verb list --verb create --verb delete --resource pods`
  - `kubectl create rolebinding dev-user-binding --role developer --user dev-user`

ClusterRole / ClusterRoleBinding:
- `kubectl create clusterrole access-node --verb "*" --resource nodes`
- `kubectl create clusterrolebinding access-node-michelle --user michelle --clusterrole access-node`

Service Accounts:
- `kubectl create token dashboard-sa` (token request API changes since 1.22/1.24)

---

## Image & Docker registry
- Private registry secret:
  - `kubectl create secret docker-registry regcred --docker-server=my.io --docker-username=my --docker-password=my --docker-email=my@org.io`
- Use `imagePullSecrets` in pod spec.

---

## Security Context & NetworkPolicies
- securityContext example:
  ```
  securityContext:
    runAsUser: 10000
    capabilities:
      add: ["MAC_ADMIN"]
  ```
- NetworkPolicy example:
  ```
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: internal-policy
    namespace: default
  spec:
    podSelector:
      matchLabels:
        name: internal
    policyTypes: [Ingress, Egress]
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
  ```

Note: Some CNIs (e.g., Flannel) do not support Network Policies.

---

## CRDs, Controllers, Operators
- CustomResourceDefinition basics and example in notes.
- Custom Controllers: https://github.com/kubernetes/sample-controller
- OperatorHub: https://operatorhub.io/

---

## Volumes / PV / PVC / StorageClass
- PV (admin), PVC (user). AccessModes: RWO, RWX, ROX, RWOP.
- PVC example: set accessModes and requests.
- StorageClass: dynamic provisioning via provisioner (e.g., `kubernetes.io/gce-pd`).

---

## Linux & Networking basics (useful commands)
- ip commands: `ip link`, `ip addr`, `ip route`
- sysctl: `cat /proc/sys/net/ipv4/ip_forward`, `sysctl -p`
- Docker internals, bridge, veth pairs, NAT via iptables

---

## CNI / Calico / Flannel / Weave / Canal
- Install Calico (example)
  ```
  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/tigera-operator.yaml
  curl -LO https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/custom-resources.yaml
  sed -i "s#192.168.0.0/16#$POD_CIDR#" custom-resources.yaml
  kubectl apply -f custom-resources.yaml
  ```

---

## CoreDNS & Cluster DNS
- Test service DNS names:
  - `curl http://web-service`
  - `curl http://web-service.apps.svc.cluster.local`
- For pod IPs, CoreDNS may not resolve by default; check Corefile in the coredns ConfigMap.

---

## Ingress & Gateway API
- Ingress uses `networking.k8s.io/v1`
- Gateway API components:
  - GatewayClass -> Gateway -> HTTPRoute
- Example Gateway + HTTPRoute snippets in notes.
- Install example NGINX Gateway CRDs:
  ```
  kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v1.5.1" | kubectl apply -f -
  kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/crds.yaml
  kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/nodeport/deploy.yaml
  ```

---

## Kubeadm & cluster setup / upgrade notes
- Install prerequisites, add apt repo for correct K8s version:
  ```
  curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
  echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
  sudo apt-get update
  sudo apt-get install -y kubelet kubeadm kubectl
  sudo apt-mark hold kubelet kubeadm kubectl
  ```
- kubeadm init example:
  ```
  POD_CIDR=10.244.0.0/16
  SERVICE_CIDR=10.96.0.0/16
  sudo kubeadm init --pod-network-cidr $POD_CIDR --service-cidr $SERVICE_CIDR --apiserver-advertise-address $PRIMARY_IP
  ```
- Upgrade flow summary (master -> nodes), using `kubeadm upgrade plan`, `kubeadm upgrade apply vX.Y.Z`, then upgrade kubelet/kubectl and restart kubelet.

---

## Multipass / Vagrant notes (macOS / Apple Silicon)
- Install: `brew install vagrant`, `brew install virtualbox`, `brew install multipass`
- Multipass tips:
  ```
  sudo launchctl stop com.canonical.multipassd
  sudo launchctl start com.canonical.multipassd
  multipass start
  multipass stop --all
  multipass start --all
  # Reboot Mac if DHCP/ssh issues persist
  ```

---

## Helm
- Helm components: CLI, charts, releases, revisions, repos
- Common commands:
  - `helm repo add <name> <url>`
  - `helm repo update`
  - `helm search repo <chart>`
  - `helm install <release> <repo/chart> [--values custom-values.yaml]`
  - `helm upgrade <release> <repo/chart> --version x.y.z`
  - `helm list -A`
  - `helm uninstall <release> -n <namespace>`
- Use `helm template` to render manifests without installing, useful to avoid CRDs or to inspect outputs.

---

## Kustomize
- Base / overlays / components layout explained.
- `kustomize build k8s/ | kubectl apply -f -` or `kubectl apply -k k8s/`
- kustomization features: resources, commonLabels, namePrefix, images, patches (strategic / json6902), generators (secrets/configmaps), transformers.
- Patch examples and patch ordering: base -> patches -> generators -> transformers.

---

## JSONPath (kubectl -o jsonpath)
- Examples:
  - `kubectl get pods -o=jsonpath='{.items[0].spec.containers[0].image}'`
  - `kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'`
  - Complex:
    ```
    kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'
    ```

---

## Useful commands / debugging tips (short)
- Events:
  - `kubectl get events --sort-by=.metadata.creationTimestamp`
  - `kubectl get events -A --sort-by=.metadata.creationTimestamp`
- Pod debug:
  - `kubectl describe pod <pod>`
  - `kubectl logs <pod> [-c <container>] [--previous]`
  - `kubectl exec -it <pod> -- /bin/sh`
  - Ephemeral container: `kubectl debug -it <pod> --image=nicolaka/netshoot --target=<container>`
- If service unreachable:
  - `kubectl get endpoints <service>`
  - `kubectl describe svc <service>`
- DNS tests:
  - `kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <service>`
- Drain/cordon nodes for maintenance:
  - `kubectl drain <node> --ignore-daemonsets`
  - `kubectl cordon <node>`
  - `kubectl uncordon <node>`

---

## crictl
- Install via apt or download from GitHub
- Examples:
  - `crictl pods`
  - `crictl ps -a`
  - `crictl images`
  - `crictl logs <container-id>`
  - `crictl exec -it <container-id> sh`

---

## Secrets, ConfigMaps & updates
- ConfigMap volumes: auto-updated (with slight delay ~1 min)
- Secret mounted as volume: updates propagate (delay)
- Secret as env or secretKeyRef: values are static for running pod; restart needed to consume new value

---

## EtcD (backup/restore)
- `ETCDCTL_API=3 etcdctl snapshot save --cacert=... --cert=... --key=... --endpoints=127.0.0.1:2379 /opt/etcd-backup.db`
- Use `etcdctl snapshot restore` to restore; requires matching TLS certs and stopping etcd/kube-apiserver appropriately.

---

## Examples: Create a user with CSR, approve, and create kubeconfig credentials
- Generate private key and CSR (openssl), base64 encode CSR, create CSR resource, approve, extract certificate, and set credentials via `kubectl config set-credentials`.

---

## Misc notes & tips (collection)
- `kubectl api-resources` and `kubectl api-versions` are useful.
- `kubectl explain <resource> --recursive=false` for quick schema.
- `source <(kubectl completion bash)` for autocomplete.
- `kubectl get pv --sort-by=.spec.capacity.storage` to sort PVs.
- Use `kubectl apply --dry-run=client -f <file>` for validation.

---

## Example snippets kept from notes

- Expose pod as service:
  ```
  kubectl expose pod messaging --port=6379 --name messaging-service
  kubectl expose deployment hr-web-app --type=NodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml > hr-web-app-service.yaml
  ```

- Create CSR and approve, then extract cert:
  ```
  kubectl certificate approve john
  kubectl get csr john -o jsonpath='{.status.certificate}' | base64 -d > john.crt
  ```

- Add a VPA example:
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

---

## Final quick checklist for exam prep
- Practice with Killer.sh (2x)
- Book exam and prepare ID
- Use Chrome during exam
- Practice kubeadm, kustomize, helm, gateway API, jsonpath, openssl, crictl, and debugging steps
- Keep a cheatsheet of common kubectl/jsonpath/awk/jq commands
- Configure VSCode, tmux, and local multipass/vagrant environments for practice

################################################################################