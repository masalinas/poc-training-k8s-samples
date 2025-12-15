# Description
Create a Pod that:

    - Uses HTTP probes
    - Fails readiness until 10 seconds after startup
    - Uses /health endpoint

## Expected Results:

    - Pod starts
    - Readiness initially fails
    - After 10 seconds, pod becomes Ready

## Steps

- **STEP_01**: Design K8s deployment file

    Write a deployment file with your objects:

    ```
    apiVersion: v1
    kind: Pod
    metadata:
    name: probe-pod
    spec:
    containers:
        - name: web
        image: nginx
        readinessProbe:
            httpGet:
            path: /
            port: 80
            initialDelaySeconds: 10
            periodSeconds: 5
        livenessProbe:
            httpGet:
            path: /
            port: 80
            initialDelaySeconds: 15
            periodSeconds: 10
    ```

- **STEP_02**: Deploy k8s objects

    Execute this command:

    ```
    $ kubectl apply -f probe-pod.yaml
    ```

- **STEP_03**: Verify

    Execute these commands:

    ```
    $ kubectl describe pod probe-pod
    $ kubectl get pods
    ```