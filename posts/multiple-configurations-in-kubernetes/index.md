<!--
.. title: Multiple configurations in kubernetes
.. slug: multiple-configurations-in-kubernetes
.. date: 2019-11-08 15:15:28 UTC-03:00
.. tags: kubernetes, linux, configuration, kubectl
.. category: kubernetes
.. link:
.. description: How to manage multiple configuration files
.. type: text
-->

It may happen to you, that you start working with 2 or more different clusters in
kubernetes. At this point, you'll want to have multiple config files, instead of
replacing `~/.kube/config`, which is fine the first few times.

In order to do this we only need to set `KUBECONFIG` env variable with the path to the kubeconfigs.

Create a `configs` folder, where the kubernetes config files will live.

```bash
mkdir -p ~/.kube/configs
```

The next thing is to add the env variable to our `.bashrc`, `.zshrc` or `.profile` file,
with the location of our configurations. The paths should be separated by a `:`.

```bash
export KUBECONFIG=$HOME/.kube/configs/gke-config:$HOME/.kube/configs/eks-config
```

Reloading our terminal with `. ~/.bashrc`, or opening a new one should pick up the changes.

### Automating the config detection

Why not automate this? So everytime we add a new kubeconfig, it's detected automatically.

Here's my attempt, place this snippet in your `.bashrc` or any other terminal file.

```bash
set_kubeconfig() {
    for entry in "$HOME/.kube/configs"/*
    do
        # Get files which do not include "skip"
        if [ -f "$entry" ] && [[ $entry != *"skip"* ]];then
            kubeconfigs="$kubeconfigs:$entry"
        fi
    done

    # Clean first colons
    kubeconfigs=${kubeconfigs#":"}
    export KUBECONFIG=$kubeconfigs
}

# Execute the function
set_kubeconfig
```

This script will get all the **files** inside `~/.kube/configs`,
which do not include `skip` in their name, and will set the `KUBECONFIG`
variable to the found files.

### Switching context and namespace

Now that our configs are detected automatically, we still have to change manually between
contexts and namespaces. I'll leave here the shortcuts

Remember that a context is a mix of [cluster, namespace, user].

#### Current configuration

```bash
kubectl config view --minify  # without minify we'll see all the configs
```

#### List contexts

```bash
kubectl config get-contexts
```

#### Swtich context

```bash
kubectl config use-context <context_name>
```

#### Switch namespace

```bash
kubectl config set-context --current --namespace=<new_namespace>
```

Find me on twitter: [@santiwilly](https://twitter.com/santiwilly)

Thanks for reading!
