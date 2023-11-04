### 1. Stworzenie namesapce i resource qouta poleceniem:
```sh
kubectl apply -f lab4.yaml
```

W wyniku otrzymamy:
```sh
$ kubectl apply -f lab4.yaml
namespace/lab4 created
resourcequota/lab4qouta created
```

### 2. Stworzenie deploymentu z 3 replikami z pliku deployment.yaml:
```sh
kubectl create -f deployment.yaml --namespace=lab4
```

### 3. Weryfikacja poleceniami:
```sh
$ kubectl get pods -n lab4
NAME                              READY   STATUS    RESTARTS   AGE
restrictednginx-f4d88994d-g84zt   1/1     Running   0          60s
restrictednginx-f4d88994d-stdbw   1/1     Running   0          60s
restrictednginx-f4d88994d-tc22p   1/1     Running   0          60s
```

```sh
$ kubectl get rs -n lab4
NAME                        DESIRED   CURRENT   READY   AGE
restrictednginx-f4d88994d   3         3         3       109s
```

```sh
$ kubectl describe deployments -n lab4
Name:                   restrictednginx
Namespace:              lab4
CreationTimestamp:      Sat, 04 Nov 2023 10:24:15 +0100
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=restrictednginx
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=restrictednginx
  Containers:
   restrictednginx:
    Image:      nginx
    Port:       <none>
    Host Port:  <none>
    Limits:
      cpu:     250m
      memory:  256Mi
    Requests:
      cpu:        125m
      memory:     64Mi
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   restrictednginx-f4d88994d (3/3 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m11s  deployment-controller  Scaled up replica set restrictednginx-f4d88994d to 3
```
```
