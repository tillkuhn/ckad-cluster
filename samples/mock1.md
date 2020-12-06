export dry="--dry-run=client -o yaml" # for kubectl run quick yaml export
export force="--force --grace-period=0" # to speed up kubectl delete xy
alias apply="kubectl apply -f"
alias kc=kubectl
alias pods="kubectl get pods"

# pod via yaml 
kc run  --image nginx:alpine  nginx-448839  $dry >ex1.yml
kc apply -f ex1.yml
kc delete pod nginx-448839  $force  

# deploy with x replicas
kc create deploy --image httpd:2.4-alpine --replicas 3 httpd-frontend $dry >dep.yml

# pod with labels
kc run --image redis:alpine --labels=tier=msg messaging

# if you fix fields in rs like image, you need to kill running pods manually to force fresh ones

# expose deployment under service name in different ns
kc expose deploy redis -n hi --port 6379 --name redis-service

# create configmap and secret from scrartch with multiple vals (secrets will implicitly be base64 encoded)
kc create cm cm-xyz --from-literal=DB_NAME=HASE --from-literal=DB_HOST=hase.ele.com 
kc create secret generic db-secret-xxdf --from-literal=DB_Host=horst --from-literal=DB_User=susi 

# create pv
kc create pv
Volume Name: pv-analytics
Storage: 100Mi
Access modes: ReadWriteMany
Host Path: /pv/data-analytics

# mock2
kc create deploy my-webapp --image=nginx --replicas=2 --dry-run=client -o yaml >q1-tmpl.yaml
kc label deploy my-webapp tier=frontend
kc expose deploy my-webapp --type=NodePort --port 80 --target-port=30083 --dry-run=client -o yaml --name front-end-service

Add a taint to the node node01 of the cluster. Use the specification below:
key:app_type, value:alpha and effect:NoSchedule
Create a pod called alpha, image:redis with toleration to node01

Apply a label app_type=beta to node node02. Create a new deployment called beta-apps with image:nginx and replicas:3. Set Node Affinity to the deployment to place the PODs on node02 only


NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
kc label nodes node02 app_type=beta

Create a new Ingress Resource for the service: my-video-service to be made available at the URL: http://ckad-mock-exam-solution.com:30093/video.

    Create an ingress resource with host: ckad-mock-exam-solution.com
path:/video

kc taint node --help