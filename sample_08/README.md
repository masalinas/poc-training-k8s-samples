# Description
Create:

- Two Pods: frontend and backend
- A NetworkPolicy that:
    - Denies all ingress to backend
    - Allows traffic only from frontend

## Expected Results

- Frontend can reach backend
- Other pods cannot

## Steps:

- **STEP_01**: some plugins to be installed:

    Requires Minikube with a CNI that supports NetworkPolicy:

    ```
    $ minikube start --cni=calico
    ```

- **STEP_02**: Deploy some pods
    Execute these commands:

    ```
    $ kubectl run frontend --image=busybox -- sleep 3600
    $ kubectl run backend --image=nginx
    ```

- **STEP_03**: 

    Write a deployment file with your objects:

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
        name: allow-frontend
    spec:
        podSelector:
            matchLabels:
            run: backend
        policyTypes:
            - Ingress
        ingress:
            - from:
                - podSelector:
                    matchLabels:
                        run: frontend
    ```

- **STEP_03**: Verify

    Execute these commands:

    ```
    $ kubectl exec frontend -- wget -O- http://backend
    $ kubectl exec backend -- wget -O- http://backend
    ```