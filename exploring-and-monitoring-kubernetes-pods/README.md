<h1>
  <span class="headline">Practical Kubernetes with kubectl and Minikube</span>
  <span class="subhead">Exploring and Monitoring Kubernetes Pods</span>
</h1>

**Learning objective:** By the end of this lesson, learners will be able to view running pods, monitor real-time Kubernetes activity, and understand pod statuses to troubleshoot issues.

## Viewing running pods

To view the pods currently running in your cluster, use the following command:

```bash
kubectl get pods
```

This will show you the **user-created pods** in your cluster, but it won't display the system pods that Kubernetes uses to manage the cluster itself.

To view **all** pods, including system pods, add the `-A` flag:

```bash
kubectl get pods -A
```

This will show both user and system pods. System pods are critical for Kubernetes' operation and help manage the cluster in the background.

You can use the `kubectl get` command to view various types of resources within your Kubernetes cluster.

To see a complete list of resource types you can query, use:

```bash
kubectl get --help
```

## Monitoring Kubernetes activity in real time

Now, let’s set up a **watch terminal** to monitor Kubernetes activity in real time. This will help you see how the cluster behaves as you make changes, such as creating new pods or deployments.

1. Open a new terminal window. Place it somewhere visible so you can easily reference it during the lesson.
2. In the new terminal, run the following command:

```bash
watch kubectl get pods
```

This will continuously refresh the output of `kubectl get pods` every 2 seconds, allowing you to monitor your cluster as it updates in real time.

We'll refer to this as the **watch terminal** for the remainder of the lesson, and you'll use it to observe the changes in your cluster.

## Understanding pod statuses and troubleshooting

When you run `kubectl get pods`, you'll notice different statuses for each pod. These statuses give you important information about the state of your pods, such as whether they're running properly, being created, or encountering issues.

To better understand the pod statuses and to troubleshoot problems, you can refer to the help command:

```bash
kubectl get pods --help
```

This will provide detailed information on pod statuses, common errors, and troubleshooting tips.
