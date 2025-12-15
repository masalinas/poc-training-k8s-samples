# Description
Create a ConfigMap containing:

- APP_ENV=production
- APP_DEBUG=false
- Inject these values as environment variables into a Pod.

## Results:

- The container prints the environment variables on startup.

## Steps

- **STEP_01**: Create ConfigMap

    ```
    $ kubectl create configmap app-config \
    --from-literal=APP_ENV=production \
    --from-literal=APP_DEBUG=false
    ```

- **STEP_02**: Design K8s deployment file

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: env-pod
    spec:
    containers:
        - name: app
        image: busybox
        command: ["/bin/sh", "-c"]
        args:
            - env; sleep 3600
        envFrom:
            - configMapRef:
                name: app-config
    ```    

- **STEP_03**: Deploy k8s objects

    Execute this command:

    ```
    $ kubectl apply -f env-pod.yaml
    ```

- **STEP_04**: Verify                

    Execute this command:

    ```
    $ kubectl logs env-pod
    ```