# CKAD Cluster Setup and Training Resources

![](https://upload.wikimedia.org/wikipedia/commons/6/67/Kubernetes_logo.svg)

## About this repo
This repo started as a fork of the [kubeadm-ansible](https://github.com/kairen/kubeadm-ansible) and spins up a Kubernetes cluster using Ansible with `kubeadm`. 

My primary goal was to use the resulting cluster to prepare for the [Certified Kubernetes Application Developer (CKAD) Program](https://www.cncf.io/certification/ckad/) to have an easy-to-(re)create environment to play around with.

The setup playbook has been tested successfully with the following configuration:

* [Kubernetes 1.19](https://kubernetes.io/docs/setup/release/notes/#v1-19-0) 
* [Docker (docker-ce) 18.06](https://docs.docker.com/engine/release-notes/)
* [Calico Networking](https://www.projectcalico.org/) intead of flannel 
* Two [Ubuntu 18.04 LTS](https://ubuntu.com/download/server) small sized machines (2 vCPU, 2 GiB RAM), one acting as master and one as worker node

 I've used servers managed by [Linux Academy Cloud Playground](https://linuxacademy.com/) as they also provide a dedicated CKAD Training, but could use any cloud provider or on premise infrastructure. Remember you need to perform some intial ssh setup before running the playbook, see *System requirements* below

## Useful resources to prepare for the CKAD exam

### Curated Links (in no particular oder)
* [Udemy CKAD Course](https://www.udemy.com/course/certified-kubernetes-application-developer/)
* [LinuxAcademy CKAD Course](https://linuxacademy.com/cp/modules/view/id/305)
* [Cloud Native Foundation CKAD | About the program Overview](https://www.cncf.io/certification/ckad/)
* [Linux Foundation FAQ: CKA and CKAD &amp; CKS (Official)](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks)
* [Linux Foundation CKAD T&amp;C DOC (official legal stuff)](https://docs.linuxfoundation.org/tc-docs/certification/lf-cert-agreement)
* [CKA and CKAD - T&amp;C DOC (firewall, sudo etc.)](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad#exam-technical-instructions)
* [Exam Browser / OS Compatibility Check](https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck)
* [CKAD exp. Cédric Moular (Good), vim tips etc. ](https://dev.to/cedricmoulard/ckad-experience-3k4o)
* [twajr/ckad-prep-notes: Huge repo with List of resources and notes for passing the exam](https://github.com/twajr/ckad-prep-notes)
* [CKAD Exercises : dgkanatsios (Good) repo](https://github.com/dgkanatsios/CKAD-exercises)
* [lucassha/CKAD-resources Github Repo: Study materials for k8s CKAD](https://github.com/lucassha/CKAD-resources)
* [The CKAD browser terminal. This is a general overview of what… | by Kim Wuestkamp | codeburst](https://codeburst.io/the-ckad-browser-terminal-10fab2e8122e)
* [Some CKAD exam experience | LinkedIn](https://www.linkedin.com/pulse/my-ckad-exam-experience-atharva-chauthaiwale/)
* [Video: How to CRUSH the CKAD Exam (10min, easy to watch)](https://www.youtube.com/watch?v=5cgpFWVD8ds)
* [Video: Muralidaran Tips on preparing for CKAD - YouTube 25min, some good hints](https://www.youtube.com/watch?v=rnemKrveZks&feature=youtu.be)

### Snippets to speed your cluster interaction (très impoortante)

```
# useful aliases
alias kc=kubectl
alias kn='kubectl config set-context --current --namespace '
alias pods="kubectl get pods"
alias ke="kubectl explain --recursive"

export dry="--dry-run=client -o yaml" # for kubectl run quick yaml export
export force="--force --grace-period=0" # to speed up kubectl delete xy

# auto complete
source <(kubectl completion bash)
complete -F __start_kubectl kc # enable also for kc alias

# tune vim tabstop, softtabstop, shiftwdith and tabs=>spaces
echo "set ts=2 sts=2 sw=2 et" > ~/.vimrc 
```

### Curated kubernetes.io deeplinks

Since you are allowed to use the offical kubernetes.io documentation it can pay off to bookmark
a couple of useful locations for code cut'n'paste, as not to waste time to search for them during the exam under time pressure.

* [kubectl Cheat Sheet - Kubernetes](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [Pod: Shell to a Running Container with empyDir volmount](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)
* [Pod: ConfigMaps Mount from a Pod | Kubernetes](https://kubernetes.io/docs/concepts/configuration/configmap/#using-configmaps-as-files-from-a-pod)
* [ Pod: Use configMapKeyRef in busybox](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#define-container-environment-variables-using-configmap-data)
* [Pod: Mount Secrets via Volumes | Kubernetes](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#create-a-pod-that-has-access-to-the-secret-data-through-a-volume)
* [Pod: Security Context, Pod or Container | Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod)
* [Pod: Volumes empyDirExample](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir-configuration-example)
* [Pod: Liveness, Readiness and Startup Probes | Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-http-request)
* [Pod: Request and Limits Mem CPU](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#meaning-of-memory)
* [Nodes+pods: Taints and Tolerations | Kubernetes](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/#concepts)
* [Nodes+Pods: Assign Pods to Nodes using Node Affinity ](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/#schedule-a-pod-using-required-node-affinity)
* [Nodes+Pods: Assign Pods2 Nodesnodelector | Kubernetes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#step-two-add-a-nodeselector-field-to-your-pod-configuration)
* [Nodes+Pods: use nodeName to assign](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename)
* [Cronjob: Creation with yaml, busybox example](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/#creating-a-cron-job)
* [Deployment: Sample with 4 replicas](https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/#scaling-the-application-by-increasing-the-replica-count)
* [Deployment: Expose as Service](https://kubernetes.io/docs/concepts/services-networking/service/#defining-a-service)
* [Deployments: Rollback history + rollout set image](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#checking-rollout-history-of-a-deployment)
* [Deployment: Updating Resources Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#updating-resources)
* [Deployments: Stateful Set + Init Container](https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/#statefulset)
* [Jobs: completions, backoffLimit](https://kubernetes.io/docs/concepts/workloads/controllers/job/#job-termination-and-cleanup)
* [Services: Expose curlpod as nodeport](https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/#exposing-the-service)
* [Service: Multiport Service Ex](https://kubernetes.io/docs/concepts/services-networking/service/#multi-port-services)
* [Network Policy | Limit Ingress Access to pod](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/#limit-access-to-the-nginx-service)
* [Network Policies | the resources (big)](https://kubernetes.io/docs/concepts/services-networking/network-policies/#networkpolicy-resource)
* [Ingress | http path rules](https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource)
* [Persistent Volumes: Create PV + PVC](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volumes)
* [PersistentVolume: Configure Podwith hostPath and pvc for Storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-pod)
* [Labels and Selectors | Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)

### Vim yaml tuning explained

You need to be fluent with either nano or vim, I choose vim. Since you're be mostly editiing yaml, it makes sense
to optimize the settings to deal with yaml files, especially when it comes to indentation

* `echo "set ts=2 sts=2 sw=2 et" > ~/.vimrc`
* [Source](https://stackoverflow.com/questions/26962999/wrong-indentation-when-editing-yaml-in-vim):
  For YAML files (...) instruct Vim to use 2 spaces for indentation, use spaces instead of tabs and
* Mark lines: `Esc+V` (then arrow keys), Copy marked lines: `y`, cut: `d`, Paste: `p` or `P`
* Delete from cursor to end of file: 'dG'

## Setup your own Cluster for Training using this repo

### System requirements

* Deployment environment must have Ansible `2.4.0+` (`pip install --user ansible`)
* Master and nodes must have passwordless SSH access. For ssh login you can easily create a keypair and add the public key to remote `~/.ssh/authorized_keys`.
 ```
    # both private and public key are placed in .secret and git-ignored 
    ssh-keygen -t rsa -b 4096 -f .secret/id_rsa  -N ""
    ssh-copy-id -i .secret/id_rsa.pub cloud_user@till1.server.com # repeat or each host
```
* For easy access you can setup an custom Host entry in your `~/.ssh/config` file
```
Host *.server.com 
  User cloud_user
  IdentityFile ~/path/to/ckad-cluster/.secret/id_rsa
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
```
  
* Since ansible needs to execute some commands with elevated privileges, you may also have to use Ansible's `--ask-become-pass` option or store it in `hosts.ini` (not recommended) 
  
### Cluster Customization

Add the system information gathered above into a file called `hosts.ini`, you can use `hosts.ini.tmpl` as a template and just adapt hostnames and ssh config

Also adapt `group_vars/all.yml` to your specified configuration.
For example, pick a different version of Kubenernetes or choose `flannel` instead of `calico`
To update docker version, check [available versions](https://download.docker.com/linux/static/stable) and update [roles/docker/defaults/main.yml](roles/docker/defaults/main.yml) accordingly.

**Note:** Depending on your setup, you may need to modify `cni_opts` to an available network interface. By default, `kubeadm-ansible` uses `eth1`. Your default interface may be `eth0`.

After going through the setup, run the `site.yaml` playbook:

```sh
$ ansible-playbook site.yaml
...
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

The playbook will download `/etc/kubernetes/admin.conf` file to `.secret/admin.conf` from master and populate it to 
`~/.kube/conf` on each node (which is the default location so you don't need to specify `KUBECONFIG`) environment variable

Verify cluster is fully running using kubectl:

```sh
$ kubectl get node
NAME                         STATUS   ROLES    AGE   VERSION
till1.server.com             Ready    master   23m   v1.17.2
till2.server.com             Ready    <none>   17m   v1.17.2
```
```
$ kubectl get po --all-namespaces
NAME                                    READY     STATUS    RESTARTS   AGE
etcd-master1                            1/1       Running   0          23m
...
```
```
$ kubectl cluster-info
Kubernetes master is running at https://172.xx.xx.xx:6443
KubeDNS is running at https://172.xx.xx.xx:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
```
$ kubectl run nginx --image=nginx
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          13s
```

### Resetting the environment

Finally, reset all kubeadm installed state using `reset-site.yaml` playbook:

```sh
$ ansible-playbook reset-site.yaml
```

## Additional features
These are features that you could want to install to make your life easier.

Enable/disable these features in `group_vars/all.yml` (all disabled by default):
```
# Additional feature to install
additional_features:
  healthcheck: false
```

### Healthcheck
This will install k8s-healthcheck (https://github.com/emrekenci/k8s-healthcheck), a small application to report cluster status.
