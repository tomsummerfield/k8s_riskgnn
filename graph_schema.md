### Graph Schema for Kubernetes

Below is a list of Kubernetes Resources (objects) and their properties.

Anything with a `-` is a list.

The below are all the nodes(vertex) in the graph

Service (allows network traffic to reach the pods)
-----------------------------------------------------
metadata:
  labels:        
    app
  name
  namespace
spec:
  type
  clusterIP
  ports:
    - name
      port
      targetPort
      protocol
  selector:
    app

Ingress (Routes traffic to the service)
----------------------------------------------------
metadata:
  name
  namespace
spec:
  backend:
    serviceName
    servicePort

Deployment (creates pods and manages them)
----------------------------------------------------
metadata:
  name
spec:
  selector:
    matchLabels:
      app
  replicas
  strategy:
    type
  template:
    metadata:
      labels:
        app
 
PersistentVolume (a storage resource that exists on the cluster)
-----------------------------------------------------
metadata:
  name
spec:
  capacity:
    storage
  storageClassName
  accessModes
  hostPath:
    path
  nfs:
    server
    path

PersistentVolumeClaim (requests for storage for pods)
------------------------------------------------------
metadata:
  name
  labels:
    type
spec:
  storageClassName
  capacity:
    storage
  accessModes:
    - ReadWriteOnce
  hostPath:
    path
  resources:
    requests:
      storage
  awsElasticBlockStore:
    volumeID
    fsType

Container (application running in the pod)
------------------------------------------------------------   
name
image
imagePullPolicy
command
readinessProbe:
  exec:
    command
  initialDelaySeconds
  timeoutSeconds
livenessProbe:
  httpGet:
    path
    port    
  initialDelaySeconds
  timeoutSeconds
resources:
  limits:
    memory
    cpu
env:
  - name
    value
    valueFrom:
      fieldRef:
        fieldPath
      secretKeyRef:
        name
        key
securityContext:
  capabilities:
    add
  allowPrivilegeEscalation
  runAsUser
ports:
  - containerPort
    name
    protocol
volumeMounts:
  - name
    mountPath
    readOnly    
volumes:
  - name
    secret:
      secretName
    emptyDir
    gcePersistentDisk:
      pdName
      fsType
    persistentVolumeClaim:
      claimName
volumeClaimTemplates:
  - metadata:
      name
    spec:
      accessModes
      resources:
        requests:
          storage
nodeSelector
backoffLimit
restartPolicy
lifecycle:
  postStart:
    exec:
      command
  preStop:
    exec:
      command
args

InitContainer (a container that runs before the main container starts)
------------------------------------------------------
name
image
imagePullPolicy
command
args        
securityContext:
  privileged
  runAsUser
  allowPrivilegeEscalation
  readOnlyRootFilesystem
volumeMounts:
  - mountPath
    name

HorizontalPodAutoscaler (scales the number of pods in a deployment)
-----------------------------------------------------
metadata:
  name
spec:
  scaleTargetRef:
    apiVersion
    kind
    name
  minReplicas
  maxReplicas
  metrics:
    - type
      resource:
        name
        target:
          type
          averageUtilization

Job (one off job that runs to completion)
-----------------------------------------------------
metadata:
  name

Namespace (isolate a group of resources within the same cluster)
-------------------------------------------------
kind
metadata:
  name

NetworkPolicy (specifies how a pod is allowed to communicate with other network entities)
--------------------------------------------------------
metadata:
  name
  namespace
spec:
  podSelector:
    matchLabels:
      app
      role
  policyTypes
  ingress:
    - from:
        - ipBlock:
            cidr
            except
        - namespaceSelector:
            matchLabels:
              project
              user
        - podSelector:
            matchLabels:
              role
              app
      ports:
        - protocol
          port
  egress:
    - to:
        - ipBlock:
            cidr
      ports:
        - protocol
          port

Pod (a group of containers that are deployed together)
-------------------------------------------------
metadata:
  name
  namespace
  labels:
    env
spec:
  securityContext:
    runAsUser
    fsGroup
  hostAliases:
    - ip
      hostnames
  volumes:
    - name
      emptyDir

PodPreset (injects information into pods at creation time)
-----------------------------------------------------
metadata:
  name
spec:
  selector:
    matchLabels:
      role
  env:
    - name
      value
  volumeMounts:
    - mountPath
      name
  volumes:
    - name
      emptyDir

Secret (stores sensitive information)
-------------------------------------------------
metadata:
  name
data:
  username
  password

PodSecurityPolicy (defines security settings for pods)
-----------------------------------------------
metadata:
  name
  annotations
spec:
  privileged
  allowPrivilegeEscalation
  requiredDropCapabilities
  volumes
  hostNetwork
  hostIPC
  hostPorts:
    - min
      max
  hostPID
  runAsUser:
    rule
  seLinux:
    rule
  supplementalGroups:
    rule
    ranges:
      min
      max
  fsGroup:
    rule
    ranges:
      min
      max
  readOnlyRootFilesystem

ClusterRole (defines permissions for a group of users)
-----------------------------------------------
metadata:
  name
rules:
  - apiGroups
    resources
    resourceNames
    verbs

ClusterRoleBinding (links accounts to clusters and grants access to all resources)
-----------------------------------------------
metadata:
  name
subjects:
  - kind
    apiGroup
    name
    namespace
roleRef:
  apiGroup
  kind
  name

Role (sets permission within a particular namespace)
---------------------------------------------------
metadata:
  name
  namespace
rules:
  - apiGroups
    resourceNames
    resources
    verbs

RoleBinding (binds a role to a user or group)
-------------------------------------------
metadata:
  name
  namespace
roleRef:
  apiGroup
  kind
  name
subjects:
  - kind
    name

ServiceAccount (provide an identity for processes that run in a pod)
-------------------------------------------------
metadata:
  name
  namespace

LimitRange (a policy to constrain resource allocation for pods/objects)
-----------------------------------------------
metadata:
  name
spec:
  limits:
    - default:
        memory
      defaultRequest:
        memory
      type
      hard:
        persistentvolumeclaims
        requests.storage
      max:
        storage
      min:
        storage

StatefulSet
----------------------------------------------
metadata:
  name
spec:
  serviceName
  replicas
  selector:
    matchLabels:
      app
  template:
    metadata:
      labels:
        app
    terminationGracePeriodSeconds

StorageClass (provides a way for administrators to describe storage classes)
----------------------------------------------
metadata:
  name
provisioner
parameters:
  type

ReplicaSet (maintain a set of replica pods running at a given time)
----------------------------------------------
metadata:
  name     
spec:
  replicas                
  selector:                    
    matchLabels:                
      app        
  template:                    
    metadata:
      labels:                
        app         

CronJob (run a job at a given time)
----------------------------------------------
metadata:
  name
spec:
  schedule
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name
              image
              imagePullPolicy
              command
          restartPolicy

DaemonSet (runs a copy of a pod on every node)
----------------------------------------------
metadata:
  name
  namespace
  labels:
    k8s-app
spec:
  selector:
    matchLabels:
      name
  template:
    metadata:
      labels:
        name
    spec:
      tolerations:
        - key
          operator
          effect
      containers:
        - name
          image
          resources:
            limits:
              memory
            requests:
              cpu
              memory
          volumeMounts:
            - name
              mountPath
      terminationGracePeriodSeconds
      volumes:
        - name
          hostPath:
            path

ConfigMap (stores configuration data for pods)
----------------------------------------------
metadata:
  name
data


