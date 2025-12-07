1. Node name attribute:
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeName: node01
  containers:
  -  image: nginx
     name: nginx

2. Search for labels
kubectl get pods --selector env=dev
kubectl get po --selector env=prod,bu=finance,tier=frontend

3. Taints & Toleration
kubectl taint nodes node01 spray=mortein:NoSchedule

apiVersion: v1
kind: Pod
metadata:
  labels:
    run: bee
  name: bee
spec:
  tolerations:
  - key: "spray"
    value: "mortein"
    operator: "Equal"
    effect: "NoSchedule"
  containers:
  - image: nginx
    name: bee
   

4. Remove taint
kubectl taint nodes controlplane <TAINT>- 

5. NodeSelector - Simple scheduling

6. Node Affinity - Complex scheduling
- Prefer/Required During Scheduling Ignore During Execution
```
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```

7. Overall allocation summarize:
 - Node affinity allocated pod with labeled node.
 - Taints & Toleration - restricting undesired pod for Nodes.

8. Resources:
- CPU
1CPU core = 1 vCPU (AWS,Azure Core,GCP Core, Hyper-thread)
100m = 0.1 CPU (minimum)
- Memory
256M/256Mi
256G/256Gi (byte/bit)

Default behaviors:
- No limits of CPU or Memory
- Always limit you resources

limit:
    - default:
        cpu:500m
      defaultRequest:
        cpu: 500m
      min:
        cpu: 200m
      max:
        cpu: 700m
      type: Container

Changes are effected only new pods

9. Static Pods
Independent way to managed K8s worker node to schedule pods
Placing manifest file in worker node.

Static pod will be shown with this prefix pattern -<NodeName>

Verify static pod location:
ps -aux | grep kubelet && cat /<path>/<to>/kubelet/config.yaml

Creating new resource will create static resources, such a static pod.


10. Priority Class - Control on pod scheduling by priority

apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority-nonpreempting
value: 1000000
preemptionPolicy: Never
globalDefault: false
description: "This priority class will not cause other pods to be preempted."

 k create priorityclass high-priority --value=100000 --preemption-policy=PreemptLowerPriority

 kubectl get pods -o custom-columns="NAME:.metadata.name,PRIORITY:.spec.priorityClassName"


11. Provision new Scheduler

 apiVersion: v1
kind: Pod
metadata:
  labels:
    run: my-scheduler
  name: my-scheduler
  namespace: kube-system
spec:
  serviceAccountName: my-scheduler
  containers:
  - command:
    - /usr/local/bin/kube-scheduler
    - --config=/etc/kubernetes/my-scheduler/my-scheduler-config.yaml
    image: registry.k8s.io/kube-scheduler:v1.34.0
    livenessProbe:
      httpGet:
        path: /healthz
        port: 10259
        scheme: HTTPS
      initialDelaySeconds: 15
    name: kube-second-scheduler
    readinessProbe:
      httpGet:
        path: /healthz
        port: 10259
        scheme: HTTPS
    resources:
      requests:
        cpu: '0.1'
    securityContext:
      privileged: false
    volumeMounts:
      - name: config-volume
        mountPath: /etc/kubernetes/my-scheduler
  hostNetwork: false
  hostPID: false
  volumes:
    - name: config-volume
      configMap: "my-scheduler.yaml"  

- AND USE IT:
---
apiVersion: v1 
kind: Pod 
metadata:
  name: nginx 
spec:
  schedulerName: my-scheduler
  containers:
  - image: nginx
    name: nginx

k get events - to verify the latest action.

12. Kube Admissions Controllers:
Component for managing K8S operation lifecycle (such Namespace, other resources etc..)

vim /etc/kubernetes/manifests/kube-apiserver.yaml

Verify plugins with kube-apiserver

ps -ef | grep kube-apiserver | grep admission-plugins

