# Description
Para lograr que un Deployment se ejecute exclusivamente en tus nodos con GPU, debes realizar dos pasos en el nodo mediante kubectl (etiquetarlo y aplicarle el taint) y luego desplegar el manifiesto YAML con la afinidad, la tolerancia y la petición de recursos.

Paso 1: Preparar el nodo en Kubernetes

Supongamos que tu nodo con tarjeta gráfica se llama nodo-gpu-01. Ejecuta estos dos comandos en tu terminal para aislarlo y marcarlo.

- **Aplicar el Taint (repulsión)**: Evita que cualquier Pod sin permiso entre en este nodo.
```shell
$ kubectl taint nodes nodo-gpu-01 gpu=true:NoSchedule
```

- **Aplicar la Label (atracción)**: Permite que nodeAffinity identifique qué nodos tienen GPU.
```shell
$ kubectl label nodes nodo-gpu-01 gpu=true
```

Paso 2: Manifiesto YAML del Deployment

Guarda el siguiente código en un archivo llamado `deployment-gpu.yaml`. Fíjate que en un Deployment, las secciones tolerations y affinity van dentro de spec.template.spec (la plantilla de los Pods)

```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-cuda-gpu
  labels:
    app: cuda-worker
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cuda-worker
  template:
    metadata:
      labels:
        app: cuda-worker
    spec:
      # 1. TOLERANCIA: Permite al Pod entrar en el nodo con el taint gpu=true:NoSchedule
      tolerations:
      - key: "gpu"
        operator: "Equal"
        value: "true"
        effect: "NoSchedule"

      # 2. AFINIDAD: Obliga al Pod a ir únicamente a nodos con la label gpu=true
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: "gpu"
                operator: "In"
                values:
                - "true"

      containers:
      - name: contenedor-cuda
        image: nvidia/cuda:12.2.0-base-ubuntu22.04
        # Mantiene el contenedor vivo ejecutando nvidia-smi en bucle para verificar la GPU
        command: ["/bin/bash", "-c", "--"]
        args: ["while true; do nvidia-smi; sleep 30; done;"]
        
        # 3. RECURSOS: Asigna 1 GPU física de NVIDIA al contenedor
        resources:
          limits:
            nvidia.com: 1
```



Paso 3: Desplegar y verificar

Aplica el archivo y comprueba que el Pod haya aterrizado en el nodo correcto y reconozca la tarjeta gráfica:

- Crear el Deployment. Ejecuta `kubectl apply -f deployment-gpu.yaml`.
- Verificar el nodo asignado. Ejecuta `kubectl get pods -o wide` y confirma que la columna `NODE` muestra `nodo-gpu-01`.
- Comprobar el acceso a la GPU. Ejecuta `kubectl logs -l app=cuda-worker` para ver la salida de nvidia-smi mostrando el modelo de tu GPU y la versión de CUDA 12.2.
