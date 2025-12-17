# Description
Create a Deployment with:

- 3 replicas
- Image: nginx:1.21
- Rolling update strategy with maxUnavailable=1
- Then update the image to nginx:1.23.

## Expected Results

- Pods are updated gradually
- At least 2 pods are always running

## Steps

- **STEP_01**: Design K8s deployment file

    Write a deployment file with your objects:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: nginx-deploy
    spec:
        replicas: 3
        strategy:
            type: RollingUpdate
            rollingUpdate:
            maxUnavailable: 1
        selector:
            matchLabels:
            app: nginx
        template:
            metadata:
            labels:
                app: nginx
            spec:
                containers:
                    - name: nginx
                    image: nginx:1.21
        ```

- **STEP_02**: Deploy k8s objects

    Execute this command:

    ```
    $ kubectl apply -f nginx-deploy.yaml
    ```

- **STEP_03**: Verify

    We are going to create a temporal fort forward connection to the new pod and check with curl command:

    ```
    $ kubectl rollout status deployment/nginx-deploy

    $ kubectl get pods
    ```    