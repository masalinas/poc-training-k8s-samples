# Description
Create a Deployment that:

- Runs an HTTP server
- Scales automatically between 1 and 5 replicas¡
- Target CPU utilization: 50%

Generate load and observe scaling behavior.

## Expected Results

- Deployment scales up under load
- Scales back down when load stops

## Steps:

- **STEP_01**: Enable metrics-server (Minikube)

    ```
    $ minikube addons enable metrics-server
    ```

- **STEP_02**: Design K8s deployment file

    Write a deployment file with your objects:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: hpa-app
    spec:
        replicas: 1
        selector:
            matchLabels:
            app: hpa-app
        template:
            metadata:
            labels:
                app: hpa-app
            spec:
                containers:
                    - name: app
                      image: k8s.gcr.io/hpa-example
                      resources:
                        requests:
                            cpu: "100m"
                            limits:
                            cpu: "500m"
    ```

- **STEP_03**: Deploy k8s objects
    Execute this command:

    ```
    $ kubectl apply -f hpa-deploy.yaml
    ```

- **STEP_04**: Create HPA
    Execute this command:

    ```
    $ kubectl autoscale deployment hpa-app \
        --cpu-percent=50 \
        --min=1 \
        --max=5
    ``` 

- **STEP_05**: Generated Load  
    Execute this command:

    ```
    $ kubectl run -it load-generator --image=busybox -- /bin/sh
    while true; do wget -q -O- http://hpa-app; done
    ```

- **STEP_05**: Generated Load  
    Execute this command:

    ```
    $ kubectl get hpa
    $ kubectl get pods
    ```