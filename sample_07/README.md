# Description
Create:

- A PersistentVolume (hostPath)
- A PersistentVolumeClaim
- A Pod that writes data to the volume

Delete the Pod and recreate it — data must persist.

## Expected Results

- Data survives Pod deletion

## Steps:

- **STEP_01**: Design K8s deployment file

    Write a deployment file with your objects:

    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
    name: pv-data
    spec:
    capacity:
        storage: 1Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: /data/pv
    ```

- **STEP_02**:

    Write a deployment file with your objects:

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: pvc-data
    spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
        storage: 1Gi
    ```

- **STEP_03**: 

    Write a deployment file with your objects:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: pv-pod
    spec:
    containers:
        - name: app
        image: busybox
        command: ["/bin/sh", "-c"]
        args:
            - echo "Hello Kubernetes" >> /data/file.txt; sleep 3600
        volumeMounts:
            - mountPath: /data
            name: storage
    volumes:
        - name: storage
        persistentVolumeClaim:
            claimName: pvc-data
    ```

- **STEP_04**: Verify

    Execute these command:

     ```
    $ kubectl delete pod pv-pod
    $ kubectl apply -f pv-pod.yaml
    $ kubectl exec pv-pod -- cat /data/file.txt
    ```