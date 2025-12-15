# Description
Create a Pod with two containers:

- nginx serving files from /usr/share/nginx/html
- A busybox sidecar container that writes the current date every 5 seconds into a shared file

Use an emptyDir volume to share data.

## Expected Results

- NGINX serves a file that updates every 5 seconds with the current date
- Both containers share the same volume

## Steps:

- **STEP_01**: Design K8s deployment file

Write a deployment file with your objects:

    ```
    apiVersion: v1
    kind: Pod
    metadata:
    name: sidecar-pod
    spec:
    containers:
        - name: nginx
        image: nginx
        volumeMounts:
            - name: shared-data
            mountPath: /usr/share/nginx/html
        - name: writer
        image: busybox
        command: ["/bin/sh", "-c"]
        args:
            - while true; do date >> /data/index.html; sleep 5; done
        volumeMounts:
            - name: shared-data
            mountPath: /data
    volumes:
        - name: shared-data
        emptyDir: {}
    ```

- **STEP_02**: Deploy k8s objects

    Execute this command:

    ```
    $ kubectl apply -f sidecar-pod.yaml
    ```

- **STEP_03**: Verify

    We are going to create a temporal fort forward connection to the new pod and check with curl command:

    ```
    $ kubectl port-forward pod/sidecar-pod 8080:80

    $ curl localhost:8080
    ```