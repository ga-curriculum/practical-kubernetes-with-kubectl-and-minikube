<h1>
  <span class="headline">Practical Kubernetes with kubectl and Minikube</span>
  <span class="subhead">Deploying Applications to Minikube</span>
</h1>

**Learning objective:** By the end of this lesson, learners will be able to deploy applications to a Minikube Kubernetes cluster, view detailed pod information, and troubleshoot basic pod issues.

## Creating a deployment

To start running a pod on your Minikube cluster, use the `kubectl create deployment` command. This will create a new deployment that runs your container:

```bash
kubectl create deployment <deployment-name> --image=<image-name>:<tag>
```

**For Example:**

```bash
kubectl create deployment hello-node --image=user/my-example:latest    **** Example Only ****
```

After running the command, take a look at your **watch terminal**. What do you see? In the status column, you’ll be able to see the state of the pod and track any changes happening in real-time.

## Pod statuses

As Kubernetes manages pods, you’ll see different statuses indicating the pod's state. Here are the most common pod statuses you might encounter:

| Status              | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| `Pending`           | Kubernetes is preparing to initialize the pod.                         |
| `ContainerCreating` | Kubernetes is setting up the container inside the pod.                 |
| `ErrImagePull`      | Kubernetes failed to pull the requested Docker image.                  |
| `Running`           | The pod is running as expected.                                        |
| `Completed`         | The pod ran successfully and finished its task.                        |
| `CrashLoopBackOff`  | The pod repeatedly failed to start and is now waiting before retrying. |
| `Terminating`       | The pod is being shut down.                                            |

## Viewing your pods

Once your deployment is running, you can check the status of your pod by running the `kubectl get pods` command:

```bash
kubectl get pods
```

You should see output similar to this:

```plaintext
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-844445fcf8-8q2ld   1/1     Running   0          4m19s
```

This shows that your pod is up and running!

## Inspecting a pod

Now, let’s take a closer look at the pod you created by using the `kubectl describe pod` command. This will show detailed information about the pod, such as its **IP address**, **container status**, and any events related to the pod's lifecycle.

To inspect a pod, run:

```bash
kubectl describe pod <pod-name>
```

For example, if the pod name is `hello-node-844445fcf8-8q2ld`, you would run:

```bash
kubectl describe pod hello-node-844445fcf8-8q2ld
```

This will display detailed information about the pod, like so:

```plaintext
Name:         hello-node-844445fcf8-8q2ld
Namespace:    default
Priority:     0
Node:         minikube/192.168.49.2
Start Time:   Tue, 26 Oct 2021 11:24:32 +0100
Labels:       app=hello-node
              pod-template-hash=844445fcf8
Annotations:  <none>
Status:       Running
IP:           172.17.0.4
IPs:
  IP:           172.17.0.4
Controlled By:  ReplicaSet/hello-node-844445fcf8
Containers:
  my-example:
    Container ID:   docker://ff7b99d1b33c03b6f771c21cbfbf995f21a1330bfca79d3b122aedfe6bc7eca6
    Image:          user/my-example:latest
    Image ID:       docker-pullable://user/my-example@sha256:fae62fdfe4419c32b8aac3b7a6e0a8845b42e9002757ea59cda0c51061c9de71
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 26 Oct 2021 11:33:04 +0100
    Last State:     Terminated
      Reason:       Error
      Exit Code:    255
      Started:      Tue, 26 Oct 2021 11:24:47 +0100
      Finished:     Tue, 26 Oct 2021 11:32:37 +0100
    Ready:          True
    Restart Count:  1
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-btxsn (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  kube-api-access-btxsn:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason          Age    From               Message
  ----    ------          ----   ----               -------
  Normal  Scheduled       13m    default-scheduler  Successfully assigned default/hello-node-844445fcf8-8q2ld to minikube
  Normal  Pulling         13m    kubelet            Pulling image "user/my-example:latest"
  Normal  Pulled          13m    kubelet            Successfully pulled image "user/my-example:latest" in 14.301234102s
  Normal  Created         13m    kubelet            Created container my-example
  Normal  Started         13m    kubelet            Started container my-example
  Normal  SandboxChanged  5m10s  kubelet            Pod sandbox changed, it will be killed and re-created.
  Normal  Pulling         5m9s   kubelet            Pulling image "user/my-example:latest"
  Normal  Pulled          5m6s   kubelet            Successfully pulled image "user/my-example:latest" in 3.569600797s
  Normal  Created         5m5s   kubelet            Created container my-example
  Normal  Started         5m5s   kubelet            Started container my-example
```

This output gives you detailed insights into the pod's health, including its containers, IP address, restart count, and any recent events (e.g., image pulling, container starting).

### Troubleshooting

- If your pod is in a `CrashLoopBackOff` state, it means Kubernetes is attempting to restart the pod after a failure. You can use the `kubectl describe pod <pod-name>` command to view error messages and investigate the cause of the failure.
- Pods in the `Pending` state often indicate that Kubernetes is waiting for resources to be allocated.

By monitoring your pods and inspecting their statuses, you'll be able to catch issues early and take the necessary steps to resolve them.
