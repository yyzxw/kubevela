# How to use controlPlaneOnly

In this section, I'll illustrate by an example how to controlPlaneOnly.

## Prerequisites

1. You have installed KubeVela
2. You can access several Kubernetes cluster(remotely or locally like `kind` or `minikube`) via `vela cluster join xxx`

## Steps

### Apply trait with controlPlaneOnly

In [control-plane-only-usecase.cue](./control-plane-only-usecase.cue), you'll see a `controlPlaneOnly` field in `attributes`, which shows that whether resources generated by trait are dispatched to the hubcluster `local`.

apply the cue file in cluster

```shell
vela def apply control-plane-only-usecase.cue
```

### Apply the application
In [app-with-control-plane-only-usecase.yaml](./app-with-control-plane-only-usecase.yaml), you'll see a application with trait `hubcluster`

Just apply the file in cluster.
```shell
kubectl apply -f app-with-control-plane-only-usecase.yaml
```

### Check the application

the application in the cluster is rendered into resources like `pod`, `hpa`.

Try to describe the pod in `cluster01`:
```shell
$ kubectl get pod
NAME                                                        READY   STATUS    RESTARTS   AGE
app-with-control-plane-only-component-01-66bd996fd7-xlwsh   1/1     Running   0          18s
```

Try to describe the pod in `cluster02`:
```shell
$ kubectl get pod
NAME                                                        READY   STATUS    RESTARTS   AGE
app-with-control-plane-only-component-01-66bd996fd7-p66rv   1/1     Running   0          18s
```

Try to describe the hpa in `local`:
```shell
$ kubectl get hpa
NAME                                       READY   STATUS    RESTARTS   AGE
app-with-control-plane-only-component-01   1/1     Running   0          18s
```

You'll see the pod is running without any errors and hpa is running in `local`.