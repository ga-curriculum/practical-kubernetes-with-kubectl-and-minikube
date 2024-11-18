<h1>
  <span class="headline">Practical Kubernetes with kubectl and Minikube</span>
  <span class="subhead">Debugging and Advanced Operations</span>
</h1>

**Learning objective:** By the end of this lesson, learners will be able to debug Kubernetes applications by accessing containers, simulating auto-healing, and using Kubernetes and Docker commands to troubleshoot issues.

## Auto-Healing

In Kubernetes, applications are designed to be resilient, meaning they automatically recover from failures. To see how this works, we’ll simulate a container failure and observe Kubernetes' auto-healing feature in action.

Before we begin, take note of the name of the `pod` running your application. We’ll need this information to compare before and after we simulate a failure.

## Accessing Containers

Since we're using Minikube, containers run inside the Minikube VM, which makes it tricky to directly access them. However, we can use Minikube’s Docker instance to execute commands and inspect the containers running in the cluster.

### Enter the Minikube VM

To begin, we need to get the ID of the Minikube container.

Run:

```bash
docker ps
```

This will list all running containers. Find the container ID of your Minikube instance, then run the following to access it:

```bash
docker exec -it <container-id> /bin/sh
```

You’re now inside the Minikube container, where you can run Docker commands to explore the containers running in the Kubernetes cluster.

### View running containers

Once inside the Minikube container, let’s see which containers are currently running. Use the following command:

```bash
docker ps -a
```

This will show all containers (running and stopped). Find the container ID of the pod you want to simulate failure for.

## Simulating a Container Failure

Now, let's simulate an error by forcefully stopping a running container. This will help us observe how Kubernetes automatically handles the situation.

To stop the container, run:

```bash
docker rm -f <container-id>
```

This will forcibly terminate the container, simulating an application crash or failure.

### Observe Kubernetes auto-healing

Go to your **watch terminal** where you have `kubectl get pods` running, and observe what happens. The pod you identified earlier will disappear, and a new pod will be automatically created by Kubernetes to maintain the desired state.

This is the auto-healing behavior of Kubernetes: It always ensures the correct number of pods are running.

## Accessing the running container directly

Want to have a look inside that single container you've got running in your Kubernetes cluster? Let's do it.

It's easy to directly access a running container in Kubernetes without going through Minikube.

To do this, run the following command:

```bash
kubectl exec -it <pod-name> /bin/sh
```

> If this doesn’t work, try using `/bin/bash` instead, depending on the shell available in the container.

We're in!

### Explore the container

Once inside the container, you can run commands as if you were on a regular Linux machine. Try some basic commands like:

```bash
ls
```

This will list the files inside the container’s file system. You can also create a new file with:

```bash
touch file.txt
```

This will create a file called `file.txt` inside the container. You’re now debugging directly inside the container!

Accessing containers like this can be useful for troubleshooting and running diagnostics on the application running inside Kubernetes.


<div class="activity discussion">
  <h2 class="title">Wrapping Up Recap</h2>
  <span class="minutes"></span>
</div>

Now that you’ve had some hands-on experience with Kubernetes:

- What aspects of Kubernetes did you find most surprising or unexpected?
- What challenges did you face during this exercise, and how did you work through them?
- Are there any areas of Kubernetes that you’d like to explore further or need more clarification on?

Feel free to share your thoughts and reflections!

