# Using ConfigMaps to Manage Configuration

## Objectives

- Create ConfigMaps from literals and files
- Mount ConfigMap data as environment variables
- Mount ConfigMap data as a volume

## Prerequisites

- A running Kubernetes cluster
- `kubectl` configured to talk to that cluster
- Basic YAML knowledge

## Files

- `configmap-pod.yaml`: reads ConfigMap values as environment variables
- `volume-pod.yaml`: mounts ConfigMap data as files
- `app.properties`: sample file used to create the file-based ConfigMap

## Step-by-Step

1. Create a ConfigMap from literals:

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_PORT=8080
```

2. Create a ConfigMap from a file:

```bash
kubectl create configmap file-config --from-file=app.properties
```

3. Inspect the ConfigMaps:

```bash
kubectl describe configmap app-config
kubectl describe configmap file-config
```

4. Create the pod that consumes ConfigMap values as environment variables:

```bash
kubectl apply -f configmap-pod.yaml
```

5. Create the pod that mounts ConfigMap data as a volume:

```bash
kubectl apply -f volume-pod.yaml
```

6. Verify the environment variable values:

```bash
kubectl exec -it config-pod -- env | grep APP
```

7. Verify the mounted file:

```bash
kubectl exec -it volume-pod -- cat /etc/config/app.properties
```

## Cleanup

```bash
kubectl delete pod config-pod volume-pod
kubectl delete configmap app-config file-config
```

## Conclusion

ConfigMaps decouple configuration from application images, so you can provide environment-specific settings without rebuilding the image.
