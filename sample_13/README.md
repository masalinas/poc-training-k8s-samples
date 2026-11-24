# Description
Configurar el contexto de kubectl para conectarse a un cluster de kubernetes

## Steps
- Paso 1: Crear configuracion del cluster de kubernetes
```shell
$ kubectl config set-cluster mi-cluster \
  --server=https://192.168.56.10:6443 \
  --certificate-authority-data=<CERT_AUTH_DATA>
```

Check:
```shell
$ kubectl config get-clusters
```

-- Paso 2: crear el usuario que va a sonectarse al cluster de kubernetes
```shell
$ kubectl config set-credentials mi-usuario \
  --client-certificate-data=<CLIENT_CERT_DATA> \
  --client-key-data=<CLIENT_KEY_DATA>
```

Check:
```shell
$ kubectl config get-users
```

-- Paso 3: Create finally the context que une cluster y usuario:
```shell
$ kubectl config set-context mi-contexto \
  --cluster=mi-cluster \
  --user=mi-usuario \
  --namespace=default
```


- Pas4: Activar el nuevo contexto:
```shell
$ kubectl config use-context mi-contexto
```

Check: 

```shell
$ kubectl config get-contexts
```

En este momento cualquier comando ejecutado con kubectl utilizara el contexto activo
