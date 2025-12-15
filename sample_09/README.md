# Description
Create a Pod that:

- Uses an init container
- Waits until a service is reachable
- Starts main app only when dependency is ready

## Expected Results

- Main container does not start until service is up

## Steps:

- **STEP_01**:

    Execute these command:

    ```
    $ kubectl run db --image=nginx
    $ kubectl expose pod db --port=80
    ```

- **STEP_02**: 

    Write a deployment file with your objects:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: init-demo
    spec:
    initContainers:
        - name: wait-for-db
        image: busybox
        command: ['sh', '-c']
        args:
            - until wget -q http://db; do echo waiting for db; sleep 5; done
    containers:
        - name: app
        image: nginx
    ```

- **STEP_03**:
    Execute this commands:

    ```
    $ kubectl describe pod init-demo
    ```