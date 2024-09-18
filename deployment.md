# manifest files needed to realized the project

for jenkins to connect with the cluster for any deployment to be made jenkins need authentication and authorization this will be achieve by creating the following 
manifest files:
- ### Create serviveaccount for Jenkins
- ### Create Role for jenkins
- ### Assign Role to serviceaccount (RoleBinding)
- ### Create a secret for jenkins to authenticate to the cluster
- ### create a manifest file for the deployment

1. create a serviceaccount **svc.yaml**

  ```svc.yaml
  apiVersion: v1
  kind: ServiceAccount
  metadata:
    name: jenkins
    namespace: webapp
  ```

  Create a namespace:

  ```
  kubectl create ns webapp
  ```
  ```
  kubectl apply -f svc.yaml
  ```
2. Create a Role **role.yaml**

```role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapp
rules:
  - apiGroups:
        - ""
        - apps
        - autoscaling
        - batch
        - extensions
        - policy
        - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - pods
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

```
kubectl apply -f role.yaml
```
3. ## Assign role to the serviceaccount (roleBinding)

```bind.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role
subjects:
- namespace: webapps
  kind: ServiceAccount
  name: jenkins
```

```
kubectl apply -f bind.yaml
```

4. ## Create a token to use for authentication by creating a secrete

```secrete.yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecrete
  annotations:
    kubernetes.io/service-account.name: jenkins
type: kubernetes.io/service-account-token
```
because we didnot provide the namespace in the manifest file for the secrete we need to pass the neame space when apply the file

```
kubectl apply -f secrete.yaml -n webapp
```
5. ## run the command to use the token created by the secrete file which we need to provide as credential to jenkins so we can authenticate with k8s

```
kubectl describe secrete mysecrete -n webapp
```

6. ## To get the k8s server _endpoint_ and _cluster name_ in our home directory do the following:

```
cd /.kube
```

```
ls 
```

```
cat config
```

the we can verify the cluster name and the server endpoint to use in our Jenkis controller to generate our jenkinsfile at the pipeline syntax

7. ## create a manifest file for the deployment of our application

   












