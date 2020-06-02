#  Ansible playbooks to build a Kubernetes cluster on Raspberry Pi

Before going further, if you don't want to build unreleased versions of Kubernetes, there are other easier and more reliable ways to run stable versions of Kubernetes on Raspberry Pi's. Mainly because they use officially released Debian packages instead of binaries. One example of that would be [rak8s](https://github.com/rak8s/rak8s) by [Chris Short](https://github.com/chris-short).

![](img/pi.jpg)

I've been part of the Kubernetes release team since 1.12 and always wanted a mini-homelab to test alpha or beta Kubernetes release candidates.

## Read the full setup / build on my [blog](https://jimangel.io/post/upstream-kubernetes-on-pi/).

### Default versions

Set in [inventory.yaml](inventory.yaml):

* [kubernetes](https://github.com/kubernetes/kubernetes) v1.19.0-beta.0
* [docker](https://github.com/docker/docker-ce) v19.03.11
* [cni](https://github.com/containernetworking/plugins) v0.8.6
* [crictl cri](https://github.com/kubernetes-sigs/cri-tools) v1.18.0

### Install Ansible
https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

### Clone repo

```
git clone https://github.com/jimangel/pre-release-k8s-on-pi.git
cd repo
```

### Update inventory.yaml

Add each of your IP addresses to a node. Feel free to add more lines if needed.

```
inventory.yaml
```

### Test connection

```
ansible all -m ping
```

### Run playbook

```
ansible-playbook playbooks/up.yaml
```

(Optional) override versions in the command line:

```
ansible-playbook playbooks/up.yaml --extra-vars "kubernetes_version=v1.18.2-beta.0"
```

### Reset everything

The following will remove all components created brining the Raspberry Pis close to the "default" image state. This will **not** uninstall docker.

```
ansible-playbook playbooks/reset.yaml
```

To include removal of docker:

```
ansible-playbook playbooks/reset.yaml --tags "all,delete-docker"
```

To reset only `kubeadm` on all nodes:

```
ansible-playbook playbooks/reset.yaml --tags "kubeadm-only"
```

Of course, the only 100% cleanup is reformatting the SD cards.