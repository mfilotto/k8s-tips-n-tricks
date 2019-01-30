# Kubernetes tips and tricks

## kubectl commands

### Official kubectl Cheat Sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### View resource usage
```
kubectl top no
kubectl top po
```
### Display logs for previous started container to debug abnormal successive restarts
`kubectl logs <my-pod> --previous`

### Display http requests made by kubectl to kube-api 
`kubectl get po -v=6`

### Extract a token from a secret
`kubectl get secret <my-secret> -n<my-namespace> -ojsonpath='{.data.token}' | base64 -d`

### Extract the content of a file from a secret (dot in file name must be escaped by \\)
`kubectl get secret <my-secret> -ojsonpath='{.data.jmxremote\\.password}' | base64 -d`

### Copy an object from one namespace to another
`kubectl get secrets <my-secret> -ojson -n<my-src-namespace> | jq '.metadata.namespace = "<my-dest-namespace>"' | kubectl create -f -`

### Clean up an helm release manually
`kubectl get deploy,sts,configmaps,secret,pvc,svc -oname -l release=<my-helm-release> | while read name; do kubectl delete $name; done`

### Watch pods
`watch kubectl get po -lrelease=<my-helm-release>`

### Find pods by date with jq
```
DEPLOYMENT_STARTDATE=`jq -n 'now'`
kubectl get po -lrelease=<my-helm-release> -ojson | jq -r --arg deployment_startdate $DEPLOYMENT_STARTDATE '.items[] | select(.metadata.creationTimestamp | fromdate | tostring > $deployment_startdate) | .metadata.name'
```

### Inject an environment variable in a deployment
`kubectl set env deployment/registry STORAGE_DIR=/local`

### Check if I'm allowed to do an action
`kubectl auth can-i exec pod`

### Suspend all cronjobs at once
`kubectl get cronjobs.batch -oname | while read name; do kubectl patch $name -p '{"spec":{"suspend":true}}'; done`

## Some recipes

### Browse google registry
https://console.cloud.google.com/gcr/images/google-containers/GLOBAL

### Ease kubectl use
![Test
](https://github.com/mhausenblas/kubectl-in-action/blob/master/img/aab-twitter.jpg)

### Decode all secret content easily with ksd
https://github.com/ashleyschuett/kubernetes-secret-decode

`kubectl get secret my-secret -o yaml | ksd`

### Read logs from all replicas at a time with stern
https://github.com/wercker/stern

### Advanced Kubernetes Objects You Need to Know
https://engineering.opsgenie.com/advanced-kubernetes-objects-53f5e9bc0c28

### Interact with kube-api like any other API
https://thenewstack.io/taking-kubernetes-api-spin/

### How to terminate a side-car container in Kubernetes Job
https://medium.com/@cotton_ori/how-to-terminate-a-side-car-container-in-kubernetes-job-2468f435ca99

### Docker Awareness in Java
- https://efekahraman.github.io/2018/04/docker-awareness-in-java
- https://blog.csanchez.org/2017/05/31/running-a-jvm-in-a-container-without-getting-killed/
- https://blogs.oracle.com/java-platform-group/java-se-support-for-docker-cpu-and-memory-limits
- https://banzaicloud.com/blog/java-resource-limits/

### Docker-in-Docker on Kubernetes
https://applatix.com/case-docker-docker-kubernetes-part-2/

### How To Back Up and Restore a Kubernetes Cluster using Heptio Ark
https://www.digitalocean.com/community/tutorials/how-to-back-up-and-restore-a-kubernetes-cluster-on-digitalocean-using-heptio-ark

### Configuring the Kubernetes CLI by using service account tokens
https://www.ibm.com/developerworks/community/blogs/fe25b4ef-ea6a-4d86-a629-6f87ccf4649e/entry/Configuring_the_Kubernetes_CLI_by_using_service_account_tokens1?lang=en

## Resources
- https://hackernoon.com/top-10-kubernetes-tips-and-tricks-27528c2d0222
- https://github.com/mhausenblas/kubectl-in-action
- https://discuss.kubernetes.io/t/kubectl-tips-and-tricks/
