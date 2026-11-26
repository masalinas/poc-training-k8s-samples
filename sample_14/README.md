# Description
Consultar cual es mi usuario, ro.se y permisos

## Steps

- Paso 1: Ver tu identidad exacta (usuario y grupos que recibe el API Server):
```shell
$ kubectl auth whoami
```

- Paso 2: Ver el contexto y usuario activo en tu `kubeconfig`:
```shell
$ kubectl config view --minify
```

- Paso 3: Listar tus permisos efectivos:
```shell
$ kubectl auth can-i --list
```

- Paso 3: Inspeccionar el `ClusterRole` `cluster-admin` y sus vinculaciones:
```shell
$ kubectl describe clusterrole cluster-admin
$ kubectl get clusterrolebinding cluster-admin -o yaml
```
