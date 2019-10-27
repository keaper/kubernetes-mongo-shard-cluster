# Cluster MongoDB en con sharding sobre Kubernetes

MongoDB en modo clúster con un shard construido sobre kubernetes. La configuración actual es:
 - 3 nodos replica set
 - 1 shard, la configuración actual está establecida en 1 shard, pero se pueden configurar hasta 4 con tantas réplicas como se necesite
 - 2 nodos MongoDB como routers

## Desplegar el cluster de Mongo


```sh
chmod +x initiate.sh
./initiate.sh
```
`initate.sh` lanzará todos los servicios dentro de k8s, pero sin salida a internet.
## Conectando al Mongo

Desde un POD
```sh
mongo mongodb://mongos1:27017
```

```sh
sh.status()
```
La salida debería ser 

```
mongos> sh.status()
--- Sharding Status ---   
...
...
shards:
        {  "_id" : "rs1",  "host" : "rs1/mongosh1-1:27017,mongosh1-2:27017,mongosh1-3:27017",  "state" : 1 }
...

```
Si se quiere hacer desde fuera, se puede configurar el servicio "mongos1-service" que utiliza NodePort para acceder al router 1. 

## Limpieza

Para borrar todos los artefactos de k8s creados
```sh
./clean.sh
```

## Diferentes configuraciones

- Editar el fichero `config`, y modificar `SHARD_REPLICA_SET` por el valor requerido.
- Crear una shard adicional requiere crear un fichero nuevo `mongo_sh_N.yaml`
- Puedes copiar/pegar el contenido de`mongo_sh_1.yaml`
- Cambiar los valores `mongosh1` por `mongoshN`  
- Cambiar los valores `rs1` por `rsN`
