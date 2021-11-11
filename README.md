# Deploying a K8s cluster with Ansible

Since the [Kubernetes Setup Using Ansible and Vagrant](https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/)
documentation is outdated and it couples Ansible and Vagrant highly, I created
this repository to uncouple both tools and allow for a cold-start spin of a Kubernetes
cluster using Ansible. Vagrant used in this repository is meant to be used
only as a testing sandbox for the Ansible Playbooks.

This repository contains:
- A Vagrantfile that spawns three VMs, one Kubernetes master node and two worker nodes.
- Two Ansible playbooks: one for the master node and one for the worker nodes.

Note that the Vagrant VMs are intended to use only for playbooks testing purposes,
but the playbooks should also work in physical machines.

## Creating the testing environment

To test the Ansible Playbooks, create the virtual machines using Vagrant:

```sh
$ vagrant up
```

Once Vagrant finishes creating the VMs, run the master playbook, followed by the node
playbook (password for user vagrant is `vagrant`):

```sh
$ ansible-playbook -i hosts -u vagrant -k kubernetes-setup/master-playbook.yml
SSH password:
...
$ ansible-playbook -i hosts -u vagrant -k kubernetes-setup/node-playbook.yml
SSH password:
...
```

To verify that the cluster was created successfully, ssh into `k8s-master` and run
kubectl get nodes:

```sh
$ vagrant ssh k8s-master
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 2.0


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Thu Nov 11 18:33:49 2021 from 192.168.50.1
vagrant@k8s-master:~$ kubectl get nodes
NAME         STATUS     ROLES                  AGE    VERSION
k8s-master   Ready      control-plane,master   5m3s   v1.22.3
node-1       NotReady   <none>                 97s    v1.22.3
```

## Running the playbooks on real machines

To run the playbooks on real machines, independently from Vagrant, change the
`host` file with the correct IP addresses and run the playbooks.


