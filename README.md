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

### Deploy Redis
We will deloy redis using an already existing helm chart mantained by https://bitnami.com/
```kubectl create ns irembo-redis```
### Add bitnami charts repo
```helm repo add bitnami https://charts.bitnami.com/bitnami```
```helm repo update``
``` helm install irembo-redis-release bitnami/redis --namespace irembo-redis``` 
This will produce the following out put

```
NAME: irembo-redis-release
LAST DEPLOYED: Sat Mar 14 23:29:53 2020
NAMESPACE: irembo-redis
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **
Redis can be accessed via port 6379 on the following DNS names from within your cluster:

irembo-redis-release-master.irembo-redis.svc.cluster.local for read/write operations
irembo-redis-release-slave.irembo-redis.svc.cluster.local for read-only operations


To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace irembo-redis irembo-redis-release -o jsonpath="{.data.redis-password}" | base64 --decode)

To connect to your Redis server:

1. Run a Redis pod that you can use as a client:

   kubectl run --namespace irembo-redis irembo-redis-release-client --rm --tty -i --restart='Never' \
    --env REDIS_PASSWORD=$REDIS_PASSWORD \
   --image docker.io/bitnami/redis:5.0.8-debian-10-r0 -- bash

2. Connect using the Redis CLI:
   redis-cli -h irembo-redis-release-master -a $REDIS_PASSWORD
   redis-cli -h irembo-redis-release-slave -a $REDIS_PASSWORD

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace irembo-redis svc/irembo-redis-release-master 6379:6379 &
    redis-cli -h 127.0.0.1 -p 6379 -a $REDIS_PASSWORD
```

Within the cluster redis can accessed at
```irembo-redis-release-master.irembo-redis.svc.cluster.local```
for read/write
and
```irembo-redis-release-slave.irembo-redis.svc.cluster.local```
for readonly as shown in the notes

### CI/CD
This repository contains a rudimentary CD/CD setup with travis which will build and deploy a new docker image whenever a tag is pushed the tag version should correspond with the chart version to ensure constencey. It assumes
that the following envs are setup on travis
```
REGISTRY_ORGANISATION - for example irembo or a username like enyachoke
REPOSITORY_NAME - the name of the repository for example irembonode
REGISTRY_USERNAME - username of account used to push images
REGISTRY_PASSWORD - password of account used to push images
```