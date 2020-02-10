# CKAD Cluster Setup and Training Resources

This is a fork of the [kubeadm-ansible](https://github.com/kairen/kubeadm-ansible) repo that spins up a Kubernetes cluster using Ansible with kubeadm.
I used it as preparation for the [Certified Kubernetes Application Developer (CKAD) Program](https://www.cncf.io/certification/ckad/) to have an easy-to-(re)create environment to deploy to.

It has been tested with [Kubernetes 1.17](https://kubernetes.io/blog/2019/12/09/kubernetes-1-17-release-announcement/) with [Calico Networking](https://www.projectcalico.org/) on two Ubuntu 18.04 small machines (2 vCPU, 2 GiB RAM) with one master and one worker node.
But is should also work for Ubuntu 16.04, CentOS and Debian Distribution (see original project).
 
 I've used servers managed by [Linux Academy Cloud Playground](https://linuxacademy.com/) as they also provide a dedicated CKAD Training, but could use any cloud provider or on premise infrastructure.

# CKAD Resources

## Snippets

```
alias kc=kubectl
alias kn='kubectl config set-context --current --namespace '

kc create deployment q1 --image=nginx --dry-run -o yaml >q1-tmpl.yaml
kc explain po; kc explain po.spec; kc explain pod.spec.volumes
```
```
$ cat <<EOF >>~/.vimrc ## tune vim
set tabstop=2
set expandtab
EOF
```
* Mark lines: Esc+V (then arrow keys)
* Copy marked lines: y
* Cut marked lines: d
* Past lines: p or P

## CKAD Resource collection

* [Practice Exam for Certified Kubernetes Application Developer (CKAD) Certification](https://matthewpalmer.net/kubernetes-app-developer/articles/ckad-practice-exam.html)
* [List of resources and notes for passing the Certified Kubernetes Application Developer (CKAD) exam.](https://github.com/twajr/ckad-prep-notes)
* [CKAD Tips](https://pnguyen.io/posts/ckad-tips/) and [CKAD Exercises](https://github.com/dgkanatsios/CKAD-exercises)
* [The CKAD browser terminal](https://codeburst.io/the-ckad-browser-terminal-10fab2e8122e) and [simulator](https://killer.sh/)
* [Candidate Handbook (official)](https://training.linuxfoundation.org/wp-content/uploads/2019/04/CKA-CKAD-Candidate-Handbook-v1.18-March-2019.pdf)
* [Important tips (official)](https://training.linuxfoundation.org/wp-content/uploads/2019/05/Important-Tips-CKA-CKAD-4.30.19.pdf)
* [Official Exam Resources](https://www.cncf.io/certification/ckad/) especially [curriculum](https://github.com/cncf/curriculum), [exam tips](https://training.linuxfoundation.org/wp-content/uploads/2020/01/Important-Tips-CKA-CKAD-01.28.2020.pdf) and [faq](https://training.linuxfoundation.org/wp-content/uploads/2020/01/CKA-CKAD-FAQ-01.28.2020.pdf)

# Cluster Setup

## System requirements

  - Deployment environment must have Ansible `2.4.0+`
  - Master and nodes must have passwordless SSH access
  
## Customization

Add the system information gathered above into a file called `hosts.ini`, you can use `hosts.ini.tmpl` as a template and just adapt hostnames and ssh config

Also adapt `group_vars/all.yml` to your specified configuration.
For example, pick a different version of Kubenernetes or choose `flannel` instead of `calico`

**Note:** Depending on your setup, you may need to modify `cni_opts` to an available network interface. By default, `kubeadm-ansible` uses `eth1`. Your default interface may be `eth0`.

After going through the setup, run the `site.yaml` playbook:

```sh
$ ansible-playbook site.yaml
...
==> master1: TASK [addon : Create Kubernetes dashboard deployment] **************************
==> master1: changed: [192.16.35.12 -> 192.16.35.12]
==> master1:
==> master1: PLAY RECAP *********************************************************************
==> master1: 192.16.35.10               : ok=18   changed=14   unreachable=0    failed=0
==> master1: 192.16.35.11               : ok=18   changed=14   unreachable=0    failed=0
==> master1: 192.16.35.12               : ok=34   changed=29   unreachable=0    failed=0

===============================================================================
kubernetes/master : Init Kubernetes cluster -------------------------------------------------------------------------------------------------------------------------------- 51.30s
kubernetes/node : Recreate kube-dns ---------------------------------------------------------------------------------------------------------------------------------------- 21.63s
commons/pre-install : Install kubernetes packages (Debian/Ubuntu) ---------------------------------------------------------------------------------------------------------- 19.56s
commons/pre-install : Install kubernetes packages (Debian/Ubuntu) ---------------------------------------------------------------------------------------------------------- 18.10s
docker : Install docker engine (Debian/Ubuntu) ----------------------------------------------------------------------------------------------------------------------------- 15.32s
docker : Install apt-transport-https --------------------------------------------------------------------------------------------------------------------------------------- 13.02s
docker : Add Docker APT repository ------------------------------------------------------------------------------------------------------------------------------------------ 8.62s
commons/pre-install : Add Kubernetes APT repository ------------------------------------------------------------------------------------------------------------------------- 7.59s
commons/pre-install : Add Kubernetes APT repository ------------------------------------------------------------------------------------------------------------------------- 7.45s
kubernetes/node : Join to Kubernetes cluster -------------------------------------------------------------------------------------------------------------------------------- 6.74s
Gathering Facts ------------------------------------------------------------------------------------------------------------------------------------------------------------- 4.60s
Gathering Facts ------------------------------------------------------------------------------------------------------------------------------------------------------------- 4.29s
commons/pre-install : Disable swappiness and pass bridged IPv4 traffic to iptable's chains ---------------------------------------------------------------------------------- 3.30s
commons/pre-install : Disable swappiness and pass bridged IPv4 traffic to iptable's chains ---------------------------------------------------------------------------------- 3.27s
docker : Copy Docker engine service file ------------------------------------------------------------------------------------------------------------------------------------ 3.12s
docker : Copy Docker environment config file -------------------------------------------------------------------------------------------------------------------------------- 2.64s
cni : Copy calico YAML files ------------------------------------------------------------------------------------------------------------------------------------------------ 2.50s
commons/pre-install : Copy kubeadm conf to drop-in directory ---------------------------------------------------------------------------------------------------------------- 2.49s
Gathering Facts ------------------------------------------------------------------------------------------------------------------------------------------------------------- 2.46s
commons/pre-install : Copy kubeadm conf to drop-in directory ---------------------------------------------------------------------------------------------------------------- 2.42s
```

The playbook will download `/etc/kubernetes/admin.conf` file to `$HOME/admin.conf`.

This may not work properly, but you can download the `admin.conf` from the master node and popuplate it to the nodes as default `~/.kube/config`

Verify cluster is fully running using kubectl:

```sh

$ export KUBECONFIG=~/admin.conf
$ kubectl get node
NAME      STATUS    AGE       VERSION
master1   Ready     22m       v1.6.3
node1     Ready     20m       v1.6.3
node2     Ready     20m       v1.6.3

$ kubectl get po -n kube-system
NAME                                    READY     STATUS    RESTARTS   AGE
etcd-master1                            1/1       Running   0          23m
...
```

## Resetting the environment

Finally, reset all kubeadm installed state using `reset-site.yaml` playbook:

```sh
$ ansible-playbook reset-site.yaml
```

# Additional features
These are features that you could want to install to make your life easier.

Enable/disable these features in `group_vars/all.yml` (all disabled by default):
```
# Additional feature to install
additional_features:
  helm: false
  metallb: false
  healthcheck: false
```

## Helm
This will install helm in your cluster (https://helm.sh/) so you can deploy charts.

## MetalLB
This will install MetalLB (https://metallb.universe.tf/), very useful if you deploy the cluster locally and you need a load balancer to access the services.

## Healthcheck
This will install k8s-healthcheck (https://github.com/emrekenci/k8s-healthcheck), a small application to report cluster status.

# Utils
Collection of scripts/utilities

