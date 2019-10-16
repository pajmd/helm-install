# helm-install

After installing helm with snap I got the following error when trying to initialize it:  
```
> helm init --canary-image --upgrade
$HELM_HOME has been configured at /home/pjmd/.helm.
Error: error installing: the server could not find the requested resource
```
The diverse solution proposed in google didn't work until I got to 
https://github.com/helm/helm/issues/3996: dazdaz 

Here are the steps I followed:
* rebuilt helm. see: https://helm.sh/docs/using_helm 
* exectuded the following commands:  
```
> kubectl delete service tiller-deploy -n kube-system
>  kubectl delete pods tiller-deploy -n kube-system --force=true --timeout=0s
>  kubectl delete deployment tiller-deploy -n kube-system --force=true --timeout=0s
> kubectl -n kube-system delete deploy tiller-deploy
> cd ~/GoProjects/src/k8s.io/helm/bin
>  ./helm init --service-account default --tiller-image gcr.io/kubernetes-helm/tiller:v2.15.0-rc.1
>  kubectl -n kube-system get pods
```
## Intermittent issues:  
```
pec.containers{tiller} readiness probe failed: Get http://172.17.0.6:44135/readiness: dial tcp 172.17.0.6:44135: connect: connection refused
```
To see the log:  
```
kubectl logs --namespace kube-system tiller-deploy-557875fd46-dtsdh
```
```
helm version
```
Should show both the clinet and server (tiller) running and therefore  
```
helm list
```
Should return 
