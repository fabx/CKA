## Kustomize

```
kustomize build k8s/
# does not apply
# outputs manifests to the console

kustomize build k8s/ | kubectl apply -f -
kubectl apply -k k8s/


kustomize build k8s/ | kubectl delete -f -
kubectl delete -k k8s/
```

https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/

### Json 6902 Vs strategic merge

#### Json 6902 patch

- Specific syntax with a patch op path value. 

```yaml
patches:
  - target:
      kind: Xxx
      name: xxx
    patch: |-
      - op: xxx
        path: xxx
        value: xxx
```

```yaml
patches:
  - target:
      kind: Deployment
      name: api-deployment
    patch: |- 
      - op: replace
        path: /spec/replicas
        value: 5
```

#### Strategic merge patch

- Same syntax as Kubernetes resources.

```yaml
```

```yaml
patches:
  - patch: |- 
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: api-deployment
    spec:
      replicas: 5
```

### Json 6902 patch inline Vs seperate file

#### Inline

```yaml
#kustomization.yaml
patches:
  - target:
      kind: Deployment
      name: api-deployment
    patch: |-
      - op: replace
        path: /spec/replicas
        value: 5
```

#### Seperate file

- kustomization is more complex 
- still have target kind name

```yaml
#kustomization.yaml
patches:
  - path: replica-patch.yaml
    target:
      kind: Deployment
      name: nginx-deployment
```

```yaml
#replica-patch.yaml
- op: replace
  patch: /spec/replicas
  value: 5
```

### Strategic merge patch inline Vs seperate file

#### Inline

```yaml
#kustomization.yaml
patches:
  - patch: |-
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: api-deployment
      spec:
        replicas: 5
```

#### Separate file

- kustomization is just the name if the separate file

```yaml
#kustomization.yaml
patches:
  - replica-patch.yaml
```

```yaml
#replica-patch.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 5
```

| Op | Required Fields | Use Case |
|---|---|---|
| add | path, value | Appending a new EnvVar or sidecar. |
| remove | path | Stripping out unwanted default settings. |
| replace | path, value | Updating a specific image tag or replica count. |
| test | path, value | Ensuring the patch only applies to a specific version. |
| move | from, path | Renaming keys within a manifest. |
| copy | from, path | Duplicating a value (e.g., from labels to annotations). |


To delete in strategic merge patch.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: memcached
          $patch: delete # -> patch command delete
```

To replace in json 6902.

```yaml
resources:
  - ../../base
  
labels:
  - pairs:
      environment: QA
    includeSelectors: false

patches:
- target:
    group: apps
    version: v1
    kind: Deployment
    name: api-deployment
  patch: |-
    - op: replace
      path: /spec/template/spec/containers/0/image
      value: caddy
```                      

### Components.

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

```yaml
#kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - ../../base
  
components:
 - ../../components/monitoring
```

```yaml
piVersion: kustomize.config.k8s.io/v1alpha1
kind: Component

resources:
  - ../../base
  - redis-depl.yaml
  - redis-service.yaml

patches: 
- path: api-patch.yaml
```

take care to recursive calls
TODO: ask gemini

### ConfigMapGenerator and SecretGenerator

secretgenerator and configgenerator in the documentation

Will generate unique secrets and configMaps and reference that unique secret or configMap in the kubernetes resource.

Adding labels to a secret with a secret generator.

```yaml
secretGenerator:
  - name: db-creds
    literals:
      - username=postgres
      - password=password1
    options:
      labels:
        app-config: my-config
```

How can i know how to do this from the documentation ?

#### Garbage collection in using kustomize.

```
kubectl apply -k ./code/k8s/ --prune -l app-config=my-config
#OK
```

### kustomize tips and tricks

#### kustomize edit

```
investigate kustomize edit
set 
add

kustomize edit set namespace production
#OK

kustomize edit add label role:admin org:kodekloud
#OK

kustomize edit add annotation team:devops version:2.0
#

kustomize edit set image nginx=memcached
#replace image nginx with memcached
#OK

kustomize edit set replicas api-deployment=8
#OK

kustomize edit add configmap domain-config --from-literal=domain=kodekloud.com --from-literal=subdomain=api
#OK

kustomize edit add secret my-secret --from-literal=username=root --from-literal=password=rootpassword123
#OK

kustomize edit add resource prometheus-depl.yaml
#OK
```

#### other tips and tricks

```
kustomize create --autodetect
#fast way to create the kustomization.yaml in a folder
```


