# objectives
All objects should be in the pizza namespace. This namespace already exists in the cluster.
The deployment should be named pizza-deployment.
The deployment should have 3 replicas.
The deployment's pods should have one container using the linuxacademycontent/pizza-service image with the tag 1.14.6  .
Run the container with the command nginx.
Run the container with the arguments "-g",".
The pods should expose port 80 to the cluster.
The pods should be configured to periodically check the /healthz endpoint on port 8081, and restart automatically if the request fails.
The pods should not receive traffic from the service until the / endpoint on port 80 responds successfully.
The service should be named pizza-service.
The service should forward traffic to port 80 on the pods.
The service should be exposed externally by listening on port 30080 on each node.

# prepare
export dry="--dry-run -o yaml" # --dry-run=client not yet supported with v1.13
export force="--force --grace-period=0"
echo "set ts=2 sts=2 sw=2 et" >~/.vimrc
alias kc="kubectl"
alias apply="kc apply -f"
alias pods="kc get pods"
. <(kubectl completion bash)
complete -F __start_kubectl  kc

# do
kc config  set-context --current --namespace pizza
kc create deploy --image=linuxacademycontent/pizza-service:1.14.6 pizza-deployment $dry >tk
kc scale --replicas=3 deploy pizza-deployment
kc explain pods.spec.containers.ports
kc expose deployment pizza-deployment --type=NodePort --target-port=80 # set nodeport later
kc expose deployment pizza-deployment  --type=NodePort --target-port=80 --dry-run --name pizza-service -o yaml # set nodePort manually

apiVersion: apps/v1
kind: Deployment
metadata:
  name: pizza-deployment
  namespace: pizza
spec:
  replicas: 3
  selector:
    matchLabels:
      app: pizza
  template:
    metadata:
      labels:
        app: pizza
    spec:
      containers:
      - name: pizza
        image: linuxacademycontent/pizza-service:1.14.6
        command: ["nginx"]
        args: ["-g", "daemon off;"]
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8081
        readinessProbe:
          httpGet:
            path: /
            port: 80
---
apiVersion: v1
kind: Service
metadata:
  name: pizza-service
  namespace: pizza
spec:
  type: NodePort
  selector:
    app: pizza
  ports:
  - protocol: TCP
    port: 80
    nodePort: 30080
