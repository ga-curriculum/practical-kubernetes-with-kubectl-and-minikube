<h1>
  <span class="headline">Practical Kubernetes with kubectl and Minikube</span>
  <span class="subhead">Introduction to Minikube and kubectl</span>
</h1>

**Learning objective:** By the end of this lesson, learners will be able to execute basic `kubectl` commands using both Minikube's built-in version and a standalone `kubectl` installation, as well as inspect and understand `kubectl` configuration.

## Introduction to Minikube

[Minikube](https://minikube.sigs.k8s.io/docs/start/) is a tool that makes it easy to run Kubernetes clusters locally. It runs a single-node Kubernetes cluster inside a virtual machine on your local machine, allowing you to experiment with Kubernetes without needing a full cloud-based cluster. Minikube is especially useful for development, testing, and learning purposes.

When you install Minikube, it includes its own version of kubectl, which is pre-configured to work seamlessly with the local Minikube cluster. You can also use a standalone kubectl installation to interact with your Minikube cluster, though Minikube's built-in version ensures compatibility.

## Minikube `kubectl` vs. Standalone `kubectl`

Minikube includes its own version of `kubectl`, which is guaranteed to be compatible with the currently installed version of Minikube. Using Minikube's built-in `kubectl` ensures that your commands will always work with your Minikube cluster.

To use Minikube's built-in `kubectl`, prepend `minikube` to your `kubectl` commands. For example:

```bash
minikube kubectl <command>
```

Alternatively, you can use a standalone installation of `kubectl` to interact with your Minikube cluster. In most cases, the standalone version of `kubectl` will also work seamlessly with Minikube. For example:

```bash
kubectl <command>
```

Both methods should work without issues when interacting with your Minikube cluster.

## Viewing `kubectl` Configuration

By default, `kubectl` communicates with the local Minikube cluster. This means you typically don't need to make any additional configuration changes. However, if you want to use `kubectl` to interact with a remote cluster (ex: a development or production cluster), you'll need to adjust the configuration.

To view the current configuration for `kubectl`, including clusters, contexts, and user information, run the following command:

```bash
kubectl config view
```

This command displays details about the clusters and contexts that `kubectl` is set up to use. For Minikube, your configuration should already be correctly set up, so no changes are necessary unless you're connecting to a different cluster.
