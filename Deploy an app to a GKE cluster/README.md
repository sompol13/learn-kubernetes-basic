# Deploy an app to a GKE cluster

In this quickstart, you deploy a simple web server containerized application to a Google Kubernetes Engine (GKE) cluster. You will learn how to create a cluster, and how to deploy the application to the cluster so that it can be accessed by users.

## Get authentication credentials for the cluster

```
gcloud container clusters get-credentials backyard-gke-tracing --region asia-southeast1 --project innovation-lab-devops
```

## Deploy an application to the cluster

```
kubectl create deployment hello-server \
    --image=us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

**kubectl get deployment hello-server -o yaml**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2023-04-18T14:09:43Z"
  generation: 1
  labels:
    app: hello-server
  name: hello-server
  namespace: default
  resourceVersion: "2110176"
  uid: 6d144e88-27a6-4a52-9202-5e5e51e2d425
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: hello-server
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hello-server
    spec:
      containers:
      - image: us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
        imagePullPolicy: IfNotPresent
        name: hello-app
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2023-04-18T14:09:57Z"
    lastUpdateTime: "2023-04-18T14:09:57Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2023-04-18T14:09:44Z"
    lastUpdateTime: "2023-04-18T14:09:57Z"
    message: ReplicaSet "hello-server-d9cdd8555" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
```

## Expose the Deployment

```
kubectl expose deployment hello-server --type LoadBalancer --port 80 --target-port 8080
```

**kubectl get service hello-server -o yaml**

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    cloud.google.com/neg: '{"ingress":true}'
  creationTimestamp: "2023-04-18T14:16:58Z"
  finalizers:
  - service.kubernetes.io/load-balancer-cleanup
  labels:
    app: hello-server
  name: hello-server
  namespace: default
  resourceVersion: "2115094"
  uid: 00ccb174-604b-416b-bfc8-30f6dd796e3a
spec:
  allocateLoadBalancerNodePorts: true
  clusterIP: 241.25.201.176
  clusterIPs:
  - 241.25.201.176
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 32109
    port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: hello-server
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - ip: 34.87.155.42
```

## Delete the application's Service

```
kubectl delete service hello-server
```
