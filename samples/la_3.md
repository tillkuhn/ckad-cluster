# objectives
Additional Information and Resources
Our company has a Kubernetes cluster that is running several pods and services. A service called oauth-provider in the gem namespace has stopped working. Our task is to investigate the service and determine what is causing the problem, save some relevant data for future analysis, and then fix the problem.

The broken service is oauth-provider. It is located in the gem namespace.
Identify which Kubernetes object is broken and causing the service to fail. Save the name of the object in the file /usr/ckad/broken-object-name.txt.
Save the object's summary data in JSON format to the file /usr/ckad/broken-object.json.
Edit the object to fix the problem.
The oauth-provider service is a NodePort service listening on port 30080, so we can test it using curl localhost:30080.
Note that there may be other objects in the cluster which appear to be broken. Our task is to identify what specifically is causing the oauth-provider service to fail. Ignore any object which doesn't affect the oauth-provider service.

# prepare
export dry="--dry-run -o yaml" # --dry-run=client not yet supported with v1.13
export force="--force --grace-period=0"
echo "set ts=2 sts=2 sw=2 et" >~/.vimrc
alias kc="kubectl"
alias apply="kc apply -f"
alias pods="kc get pods"
. <(kubectl completion bash)
complete -F __start_kubectl  kc
kc config  set-context --current --namespace gem


# do
$ kc get all
$ kc get pods --show-labels
NAME      READY   STATUS             RESTARTS   AGE   LABELS
bowline   0/1     ImagePullBackOff   0          60m   role=oauth
hitch     0/1     ImagePullBackOff   0          60m   role=ws

echo bowline >/usr/ckad/broken-object-name.txt

