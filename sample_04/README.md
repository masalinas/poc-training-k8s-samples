# Description
Expose an NGINX Deployment using:

- ClusterIP
- Change it to NodePort
- Access it from your browser using Minikube

## Expected Result

- Service reachable via minikube service

## Steps

- **STEP_01**: Deploy services

     Execute these commands:

    ```
    $ kubectl create deployment nginx --image=nginx
    $ kubectl scale deployment nginx --replicas=2
    ```

- **STEP_02**: Deploy services

    Execute this command:

    ```
    $ kubectl expose deployment nginx --port=80
    ```

- **STEP_03**: Access

    Execute this command:

    ```
    $ minikube service nginx
     ```