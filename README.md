# vagrant-kubernetes

## Description

Simple way to get Kubernetes running on local machine. Just clone, cd and do:

```
vagrant up
```

If vagrant provider is VirtualBox that should be sufficient. Else you may want to modify ansible inventory to reflect correct paths for ssh keys (replace 'virtualbox' with eg. 'libvirt').

Once vagrant boxes are up you may want to do something like:

```
vagrant ssh node1
kubectl get nodes
```
Etc.

This uses CentOS 7 as a base. Probably would work on Fedora as well.

Provides 3 nodes, etcd running on all of them in cluster, First node is master, other two - kubelets. It's trivial to tweak Vagrantfile and ansible inventory to add as much nodes as desired.

Goal is to develop Ansible roles suitable for provisioning Kubernetes clusters in real live environments where using something like CoreOS is not always an option.

## To do

- [ ] Cleanup and finalize etcd role
- [ ] Cleanup etcd module
- [ ] Add metadata and descriptions for all roles
- [ ] Add tests
