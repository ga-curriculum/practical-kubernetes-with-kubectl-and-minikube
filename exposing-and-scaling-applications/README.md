<h1>
  <span class="headline">Practical Kubernetes with kubectl and Minikube</span>
  <span class="subhead">Exposing and Scaling Applications</span>
</h1>

**Learning objective:** By the end of this lesson, learners will be able to expose a Kubernetes service to external traffic, scale the number of application replicas up and down, and verify the system's behavior during scaling operations.

## Exposing a service

In Kubernetes, your pods are running in a private cluster and cannot be accessed externally by default. To allow external traffic to reach your application, you need to expose a port.

To expose the `hello-node` application, run:

```bash
kubectl expose deployment hello-node --type=NodePort --port=3000
```

This command exposes port 3000 of the pod inside the Kubernetes network and creates a service to route traffic to it.

You can confirm the service has been created by running:

```bash
kubectl get services
```

You should see something like this:

```plaintext
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-node   NodePort    10.107.211.49   <none>        3000:32201/TCP   10m
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          23m
```

Here, the `hello-node` service is exposed on port `32201` on your Minikube cluster. Kubernetes has mapped the internal pod port `3000` to the external `32201`.

## Taking a closer look at the service

To see more details about the service, run:

```bash
kubectl describe service hello-node
```

This will show detailed information about the service, like this:

```plaintext
Name:                     hello-node
Namespace:                default
Labels:                   app=hello-node
Annotations:              <none>
Selector:                 app=hello-node
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.107.211.49
IPs:                      10.107.211.49
Port:                     <unset>  3000/TCP
TargetPort:               3000/TCP
NodePort:                 <unset>  32201/TCP
Endpoints:                172.17.0.4:3000
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

The `NodePort` type means that your service is exposed externally on port `32201`. This allows you to access the service through Minikube's external IP.

## Accessing the application through Minikube proxy

To access the service in your browser, use Minikube's proxy with the following command:

```bash
minikube service hello-node
```

The output will show the URL to access the application:

```plaintext
| NAMESPACE |     NAME      | TARGET PORT |            URL            |
|-----------|---------------|-------------|---------------------------|
| default   | hello-node    |        3000 | http://192.168.49.2:32201 |
|-----------|---------------|-------------|---------------------------|

🎉  Opening service default/hello-node in default browser...
```

This will open the `hello-node` service in your default browser, and you can view the running application!

## Scaling up

Once your application is running, you might want to scale it to handle more traffic by increasing the number of replicas (pods).

To scale up, use the following command to set the number of replicas to **3**:

```bash
kubectl scale deployments/hello-node --replicas=3
```

You can check the status of the pods with:

```bash
kubectl get pods
```

Now you should see three pods running:

```plaintext
NAME                             READY   STATUS    RESTARTS   AGE
nginx-example-5bf5d67944-8kz54   1/1     Running   0          72s
nginx-example-5bf5d67944-gtwg4   1/1     Running   0          29m
nginx-example-5bf5d67944-pc67g   1/1     Running   0          72s
```

The **AGE** column will show how long each pod has been running. Kubernetes will automatically balance the incoming traffic to these pods, distributing the load.

You can refresh your browser to see that requests are now being routed to different containers each time. Kubernetes is performing load balancing between the **3 pods**.

## Scaling down

If the number of users decreases and you want to reduce the number of running pods, you can scale down the deployment by using the same `kubectl scale` command, but with the `replicas` flag set to `1`:

```bash
kubectl scale deployments/hello-node --replicas=1
```

After running this command, check the status of the pods:

```bash
kubectl get pods
```

In your **watch terminal**, you’ll see some of the pods will go into the `Terminating` state as they are gracefully shut down.

Refresh your browser several times to observe that requests continue to be handled smoothly, even as Kubernetes terminates the excess pods. Kubernetes ensures that traffic is only routed to running pods during this process.

If a pod takes longer than 30 seconds to shut down, Kubernetes will forcefully terminate it to free up resources.

After scaling down, your `kubectl get pods` output should look like this:

```plaintext
NAME                              READY   STATUS    RESTARTS   AGE
nginx-example-5bf5d67944-gtwg4    1/1     Running   0          53m
```

This means you've successfully scaled down the number of running pods!
