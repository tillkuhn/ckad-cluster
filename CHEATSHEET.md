# Commands shared in the PPT
kubectl run nginx --image=nginx   (deployment)
kubectl run nginx --image=nginx --restart=Never   (pod)
kubectl run nginx --image=nginx --restart=OnFailure   (job)  
kubectl run nginx --image=nginx  --restart=OnFailure --schedule="* * * * *" (cronJob)

kubectl run nginx -image=nginx --restart=Never --port=80 --namespace=myname --command --serviceaccount=mysa1 --env=HOSTNAME=local --labels=bu=finance,env=dev  --requests='cpu=100m,memory=256Mi' --limits='cpu=200m,memory=512Mi' --dry-run -o yaml - /bin/sh -c 'echo hello world'

kubectl run frontend --replicas=2 --labels=run=load-balancer-example --image=busybox  --port=8080
kubectl expose deployment frontend --type=NodePort --name=frontend-service --port=6262 --target-port=8080
kubectl set serviceaccount deployment frontend myuser

# change namespace of current context 
kubectl config set-context --current --namespace=marketing
# or vi ~/.kube/config

# Create a service for an redis deployment, which serves on port 80 and connects to the containers on port 8000. default is ClusterIP
kubectl expose deployment redis --port=80 --target-port=8000 --name redis-svc

# quick create configmap or secret with 2 key/value pairs
kubectl create cm klaus2 --from-literal=k1=dddd --from-literal=k2=wolf
kubectl create secret generic supergeheim --from-literal=DB_Host=sss --from-literal=DB_P=ruti 

# Create cronjob
kubectl create cronjob nginx --image=nginx --schedule="* * * * *"  

