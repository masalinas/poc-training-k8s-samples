# Description
Create:

- A Deployment with 3 replicas
- A PodDisruptionBudget that ensures at least 2 pods are always available

Attempt to evict pods.

## Expected Results

- Evictions blocked when violating PDB

## Steps:

- **STEP_01**:

    ```
    $ kubectl create deployment web --image=nginx
    $ kubectl scale deployment web --replicas=3
    ```

- **STEP_02**: 

    Write a deployment file with your objects:

    ```yaml
    apiVersion: policy/v1
    kind: PodDisruptionBudget
    metadata:
        name: web-pdb
    spec:
        minAvailable: 2
        selector:
            matchLabels:
                app: web
    ```

- **STEP_03**:  Apply
    Execute this commands:

    ```
    $ kubectl apply -f pdb.yaml
    ```

- **STEP_04**: Test eviction
    Execute this commands:

    ```
    $ kubectl drain minikube --ignore-daemonsets --delete-emptydir-data
    ```