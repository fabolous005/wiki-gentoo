<!-- source: https://wiki.gentoo.org/wiki/Kubernetes | group: Gentoo Wiki (Main) | wiki-title: Kubernetes -->
---
title: Kubernetes
url: https://wiki.gentoo.org/wiki/Kubernetes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-23"
fingerprint: af1ac15f7e679b95
license: CC BY-SA 4.0
---

# Kubernetes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Kubernetes**, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

This article outlines two approaches for running a Kubernetes cluster:

- via **[minikube](https://minikube.sigs.k8s.io/)** to get started, great for learning Kubernetes from a user perspective (easy), or
- via **[kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)**, an admin tool for cluster nodes that is part of Kubernetes itself (moderate to difficult).

Note that other approaches are available, like [kOps](https://kops.sigs.k8s.io/) to set up a cluster when a cloud API is available, or [Kubespray](https://kubespray.io/) to set up a cluster using [Ansible](https://wiki.gentoo.org/wiki/Ansible).

## Kubernetes architecture

TBD

Kubernetes groups containers that make up an application into logical units for easy management and discovery and runs them on a Kubernetes cluster. A Kubernetes cluster is made of [two groups of components](https://kubernetes.io/docs/concepts/overview/components/), namely control plane components, which are used to manage the cluster, and node components, which run the workers hosting the applications. Kubernetes requires a [container runtime interface](https://kubernetes.io/docs/concepts/architecture/cri/) (CRI), which is the mechanism for running a container.

## Kubernetes via `minikube`

`minikube` requires a "driver" in which to run itself and containers. Docker is one such driver, and this example will use it, but just note that other options exist.

`root #``emerge --ask sys-cluster/minikube sys-cluster/kubectl app-containers/docker app-containers/docker-cli`
You will need to add your user to the `docker` group.

Then,

`user $``minikube start`
You now have a single-node cluster! For example, you should now be able to run `kubectl get pods` to show the current running pods in the `default` namespace. (This will be empty, as there are none to start with.)



## Kubernetes via kubeadm

In the following instruction we are going to set up single-master multiple-node cluster. It consists of:

1. Containerd as container runtime - on every node
2. Kubelet as node (and master) agents - on every node
3. Kubeadm to set up cluster - on every node
4. cni-plugins for networking (Optionally) - on every node
5. Kubectl to control them all - on the user's machine

The following components will be automatically installed as kubelet and run as containers:

1. kube-apiserver
2. kube-controller-manager
3. kube-scheduler
4. etcd
5. kube-proxy (Optionally)
6. CoreDNS (Optionally)

Cilium will be considered as a viable option to Kubernetes networking and kube-proxy.

### Preparing the nodes

Pick a suitable CRI driver, and follow the instructions on its page, e.g. [containerd](https://wiki.gentoo.org/wiki/Containerd). Pay special attention to the correct configuration of the cgroup driver of the CRI.

Install the main kubeadm-based bootstrap components.

`root #``emerge --ask sys-cluster/kubectl sys-cluster/kubeadm`
#### kubelet OpenRC

**`/etc/conf.d/kubelet`**

**Adapting OpenRC kubelet service to kubeadm**

```
###
# Kubernetes Kubelet (worker) config
# This configuration example is tuned for kubeadm
# Feel free to modify
# If exists kubeadm-flags.env, then source it to get
# environment variable ${KUBELET_KUBEADM_ARGS}
if [ -e /var/lib/kubelet/kubeadm-flags.env ]; then
  source /var/lib/kubelet/kubeadm-flags.env;
fi
# Pre-defined command-line args for kubeadm
command_args="--config /var/lib/kubelet/config.yaml \
        --kubeconfig /etc/kubernetes/kubelet.conf \
        --bootstrap-kubeconfig /etc/kubernetes/bootstrap-kubelet.conf \
        ${KUBELET_KUBEADM_ARGS}"
```
#### systemd

For [systemd](https://wiki.gentoo.org/wiki/Systemd), kubeadm uses what is called a "drop-in" systemd file. See [this page in the Kubernetes documentation](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/kubelet-integration/#the-kubelet-drop-in-file-for-systemd) for more info.

Usually, the drop-in file is created during the installation of the `kubeadm` package in /usr/lib/systemd. This is currently missing, so find the appropriate content in the documentation above, and place it in /etc/systemd.

**`/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`**

**Add overrides to kubelet.service for kubeadm integration**

```
[Service]
Environment="KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf"
Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
# This is a file that "kubeadm init" and "kubeadm join" generates at runtime, populating the KUBELET_KUBEADM_ARGS variable dynamically
EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
# This is a file that the user can use for overrides of the kubelet args as a last resort. Preferably, the user should use
# the .NodeRegistration.KubeletExtraArgs object in the configuration files instead. KUBELET_EXTRA_ARGS should be sourced from this file.
EnvironmentFile=-/etc/sysconfig/kubelet
ExecStart=
ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS
```
Enable the unit, but do not start it.

`root #``systemctl enable kubelet.service`
Starting it now would result in a crash loop because essential configuration is missing. kubeadm will configure and start the service later, and issue a warning if the service has not been enabled yet.

### Basic configuration

To initialize a cluster with kubeadm, we need to pass some basic configuration.
It is recommended to write configuration manifests to a file and pass the file name via `--config`.

Supported kinds are:

- `InitConfiguration` for node-specific runtime configuration
- `ClusterConfiguration` for settings regarding the whole cluster
- `KubeletConfiguration` for settings passed to every kubelet of the cluster
- `KubeProxyConfiguration` for kube-proxy (if used)

Either `InitConfiguration` or `ClusterConfiguration` is required, so a minimum configuration could look like this:

**`/etc/kubernetes/kubeadm-config.yaml`**

**kubeadm config**

```
---
apiVersion: kubeadm.k8s.io/v1beta4
kind: ClusterConfiguration
kubernetesVersion: v1.35.0
```
In Kubernetes 1.31+, kubeadm asks the CRI for the cgroup driver to use.
For CRIs that do not support the RuntimeConfig CRI API, the cgroup driver used by the kubelet should be specified in the `KubeletConfiguration`.
Kubernetes 1.37+ will drop support for CRIs that do not support the RuntimeConfig CRI API<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.
Support was added to [containerd](https://wiki.gentoo.org/wiki/Containerd) in version v2.0<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, and to [CRI-O](https://wiki.gentoo.org/index.php?title=CRI-O&action=edit&redlink=1) in version 1.28<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

**`/etc/kubernetes/kubeadm-config.yaml`**

**Specifying a cgroup driver**

```
<omitted>
---
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: cgroupfs # if omitted, kubeadm defaults to systemd
```
### Initializing the cluster

Run `kubeadm init` to initialize the cluster:

`root #``kubeadm init --config /etc/kubernetes/kubeadm-config.yaml`
\<omitted>
Your Kubernetes control-plane has initialized successfully!
To start using your cluster, you need to run the following as a regular user:

The [official troubleshooting page](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/) lists problems that might occur during `kubeadm init`, and how to solve them.

### Adding nodes to the cluster

### Removal

Before removing a node from its cluster, make sure there are no pods left on the node.

`user $``kubectl drain "$(hostname -s)" --ignore-daemonsets`
The node should then be safe to remove.

`user $``kubectl delete node "$(hostname -s)"`
Finally, stop all Kubernetes containers and clean up the file system.

`root #``kubeadm reset`
Some running containers might survive the reset. They can usually be identified via the `NAMESPACE` field in the output of

`root #``crictl ps -a`
If *every* listed container belongs to the former node, they can be removed with `crictl rm -a`; otherwise remove them separately.

It is now safe to remove the system packages.

`root #``emerge --ask --depclean --verbose sys-cluster/kubeadm sys-cluster/kubectl sys-cluster/kubeadm`
Even after running `kubeadm reset`, some traces of `kubeadm init` or `kubeadm join` and the following operation of the cluster will be left on the host.
To be extra thorough, the user might want to review and clean up the contents of /etc/kubernetes and /var/lib/kubelet.

In any case, to avoid problems in case this host will be used as a Kubernetes node again later, remove /etc/cni/net.d.

`root #``rm -rf /etc/cni/net.d`
Depending on the selected CNI, some obsolete network interfaces might still remain. The following example has interfaces left that were created by Cilium.

`root #``ip link````
# omitted: loopback and physical interfaces...
6: cilium_net@cilium_host: <BROADCAST,MULTICAST,NOARP,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP mode DEFAULT group default 
    link/ether 4e:e8:a1:fb:54:43 brd ff:ff:ff:ff:ff:ff
7: cilium_host@cilium_net: <BROADCAST,MULTICAST,NOARP,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 22:a9:f3:a1:16:f4 brd ff:ff:ff:ff:ff:ff
8: cilium_vxlan: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UNKNOWN mode DEFAULT group default 
    link/ether 36:5c:a1:7b:ad:ea brd ff:ff:ff:ff:ff:ff
101: lxcf7ef2b630eac@if100: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 7e:26:ac:b0:03:8e brd ff:ff:ff:ff:ff:ff link-netns cni-4c7a741d-7fdc-208c-e035-fcc76352ba7c
```
They should be safe to remove.

`root #``ip link delete cilium_net``root #``ip link delete cilium_vxlan`
The last remaining interface in the above example is in its own networking namespace; this was used to isolate a pod's network from other pods' and the host's networks. Removing the networking namespace will remove the interface, too.

`root #``ip netns delete cni-4c7a741d-7fdc-208c-e035-fcc76352ba7c`
## See also

- [Docker](https://wiki.gentoo.org/wiki/Docker) — a [container](<https://en.wikipedia.org/wiki/Container_(virtualization)>)-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system
- [LXD](https://wiki.gentoo.org/wiki/LXD) — a system container manager
- [Podman](https://wiki.gentoo.org/wiki/Podman) — a daemonless container engine for developing, managing, and running [OCI Containers](https://opencontainers.org/), aiming to be a drop-in replacement for much of [Docker](https://wiki.gentoo.org/wiki/Docker)
