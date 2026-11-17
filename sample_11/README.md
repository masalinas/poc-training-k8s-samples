## Ejecutar kustomize
Para previsualizar cómo Kustomize fusiona los archivos base con los parches de desarrollo sin aplicar cambios en el clúster:

```shell
kubectl kustomize ./overlays/dev/
```

Para aplicar directamente el entorno de producción en el clúster:

```shell
kubectl apply -k ./overlays/prod/
```
