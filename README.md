# Kubernetes tips and tricks

### Browse google registry
https://console.cloud.google.com/gcr/images/google-containers/GLOBAL

### Display http requests made by kubectl to kube-api 
`kubectl get po -v=6`

### Extract a token from a secret
`kubectl get secret <my-secret> -n<my-namespace> -ojsonpath='{.data.token}' | base64 -d`

### Extract the content of a file from a secret (dot in file name must be escaped by \\)
`kubectl get secret <my-secret> -ojsonpath='{.data.jmxremote\\.password}' | base64 -d`

### Copy an object from one namespace to another
`kubectl get secrets <my-secret> -ojson -n<my-src-namespace> | jq '.metadata.namespace = "<my-dest-namespace>"' | kubectl create -f -`

### Clean up an helm release manually
`kubectl get configmaps,secret,pvc,svc,deploy,sts -oname -l release=<my-helm-release> | while read name; do kubectl delete $name; done`

### Watch pods
`watch kubectl get po -lrelease=<my-helm-release>`

### Inject an environment variable in a deployment
`kubectl set env deployment/registry STORAGE_DIR=/local`

### Check if I'm allowed to do an action
`kubectl auth can-i exec pod`
