<h1>
  <span class="headline">Practical Kubernetes with kubectl and Minikube</span>
  <span class="subhead">Starting and Exploring a Minikube Cluster</span>
</h1>

**Learning objective:** By the end of this lesson, learners will be able to start a Minikube cluster, verify its operation, access the Kubernetes dashboard, and retrieve key information about the cluster using `kubectl`.

## Starting the Minikube cluster

To launch a local Kubernetes cluster using Minikube, use the following command:

```bash
minikube start
```

This command initializes a Minikube container on your local machine, which will manage your Kubernetes cluster.

### Verifying Minikube startup

After starting Minikube, you can confirm that the Minikube container is running by listing all Docker containers with:

```bash
docker ps -a
```

You should see an entry for the Minikube container in the output, indicating it is running.

## Exploring the Kubernetes dashboard

Once your Minikube cluster is running, you can access the Kubernetes dashboard to get a visual overview of the cluster.

Open the dashboard by running:

```bash
minikube dashboard
```

This command launches a web-based interface in your default browser, showing:

- The current state of your cluster.
- The number of pods, services, and other resources.
- Metrics and performance details.

The dashboard provides an intuitive way to explore your cluster and monitor its health.

<br>

<img src="./assets/kubernetes-dashboard.png" alt="Kubernetes Dashboard" style="width:800px;"/>

<br>

## Viewing cluster information

To gather more in-depth information about your Minikube cluster, use the following command:

```bash
kubectl cluster-info
```

This command provides details such as the addresses of the Kubernetes control plane and other services running in the cluster.
