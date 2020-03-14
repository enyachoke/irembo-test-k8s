### Setup Namespace for the app
```kubectl create namespace irembo```
### Create custom values file
```touch custom-values.yaml ```
customise the file as needed the included custom-values.yaml shows how to enable ingress
### Deploy using included helm chart
```helm install irembonode irembo-node --namespace irembo```
You should see an output similar to this
```
NAME: irembonode
LAST DEPLOYED: Sat Mar 14 22:53:05 2020
NAMESPACE: irembo
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace irembo -l "app.kubernetes.io/name=irembo-node,app.kubernetes.io/instance=irembonode" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace irembo port-forward $POD_NAME 8080:80
```
or with custom values

```helm install irembonode irembo-node --namespace irembo -f custom-values.yaml```

Which will will create an ingress and produce the output below

```
NAME: irembonode
LAST DEPLOYED: Sat Mar 14 22:56:26 2020
NAMESPACE: irembo
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  https://irembotest.emmanuelnyachoke.com/
```
The above example is live at https://irembotest.emmanuelnyachoke.com  my personal Kubernetes cluster deployed on linode.

