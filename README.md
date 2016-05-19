# vagrant-kubernetes

## Description

Simple way to get Kubernetes running on local machine. Just clone, cd and do:

```
vagrant up
```

If vagrant provider is VirtualBox that should be sufficient. Else you may want to modify ansible inventory to reflect correct paths for ssh keys.

Once vagrant boxes are up you may want to do something like:

```
vagrant ssh node1
kubectl get pods
```
Etc.

This uses CentOS 7 as a base. Probably would work on Fedora as well.

Goal is to develop Ansible roles suitable for provisioning Kubernetes clusters in real live environments where using something like CoreOS is not always an option.

## To do

- [ ] Cleanup and finalize etcd role
- [ ] Cleanup etcd module
- [ ] Add metadata and descriptions for all roles
- [ ] Add tests
