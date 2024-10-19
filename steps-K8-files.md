# Create Service Account, Role & assign that Role to Service Account and create a Secret for that Service Account & generate a Token.


#### First create namespace in K8s cluster
kubectl create namespace webapps


#### Creating Service Account (vi sa.yml)
apiVersion: v1
kind: ServiceAccount
metadate:
  name: jenkins
  namespace: webapps

#### Applying the command to create the Service Account
kubectl apply -f sa.yml


####  Creating Role for SA (vi role.yml)
apiVersion: rbac.authorization.k8.io/v1
kind: Role
metadate:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
    - ""
    - apps
    - autoscaling
    - batch
    - extensions
    - policy
    - rbac.authorization.k8.io
    resources:
      - pods
      - componentstatustes
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

#### Applying the command to create the role
kubectl apply -f role.yml


#### Bind the role to the Service Account (vi assign.yml)
apiVersion: rbac.authorization.k8.io/v1
kind: RoleBinding
metadate:
  name: app-rolebinding
  namespace: webapps
roleRef:
  apiGroup: rbac.authorization.k8.io
  kind: Role
  name: app-role
subjects:
- namespace: webapps
  kind: ServiceAccount
  name: jenkins

#### Applying the command to assign the role to SA
kubectl apply -f assign.yml


#### Creating token for the Service Account (vi secret.yml)
apiVersion: v1
kind: Secret
type: kubernetes.io/secret-account-token
metadate:
  name: mysecret
  annotations:
    kubernetes.io/secret-account-token = jenkins

#### Applying the command to assign the secret to SA
kubectl apply -f secret.yml -n webapps

#### To view the token inside a namespace
kubectl -n webapps describe secret mysecret


#### .kube/config file to get K8s cluster info
cd ~/.kube