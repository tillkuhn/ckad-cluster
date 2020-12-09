```
cloud_user@ip-10-0-1-101:~$ kc set image deployment candy-deployment candy-ws=linuxacademycontent/candy-service:3 --record
deployment.extensions/candy-deployment image updated

cloud_user@ip-10-0-1-101:~$ kc rollout status deployment candy-deployment
Waiting for deployment "candy-deployment" rollout to finish: 1 out of 2 new replicas have been updated...


cloud_user@ip-10-0-1-101:~$ kubectl rollout history deployment/candy-deployment
deployment.extensions/candy-deployment
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment candy-deployment candy-ws=linuxacademycontent/candy-service:3 --record=true
```

kubectl rollout undo deployment/candy-deployment
kubectl rollout status deployment/candy-deployment
