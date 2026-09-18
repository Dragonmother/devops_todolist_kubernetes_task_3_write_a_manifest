# Kubernetes ToDo App Instructions

## Apply manifests

Apply the namespace first:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Apply the application and testing Pods:

```bash
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
```

Apply the Service:

```bash
kubectl apply -f .infrastructure/todoapp-service.yml
```

## Check the Pods

```bash
kubectl get pods -n todoapp
```

## Check the Service

```bash
kubectl get service -n todoapp
```

## Test using port-forward

Run:

```bash
kubectl port-forward pod/todoapp 8000:8000 -n todoapp
```

Open the following URLs in a browser:

- http://localhost:8000/api/readiness/
- http://localhost:8000/api/liveness/

The readiness endpoint should return:

```text
Ready
```

The liveness endpoint should return:

```text
Alive
```

## Test using busyboxplus:curl

Get the application Pod IP:

```bash
kubectl get pod todoapp -n todoapp -o wide
```

Use the displayed Pod IP in the following commands:

```bash
kubectl exec busybox -n todoapp -- curl http://<POD_IP>:8000/api/readiness/
```

```bash
kubectl exec busybox -n todoapp -- curl http://<POD_IP>:8000/api/liveness/
```

Expected responses:

```text
Ready
```

and:

```text
Alive
```

## Delete the resources

```bash
kubectl delete -f .infrastructure/
```