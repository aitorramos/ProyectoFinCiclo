# ProyectoFinCiclo

## Despliegue y configuración de Mongo con interfaz web con Docker

### 1. Descarga

Descarga el documento comprimido del cluster que aparece en al principio del repositorio.


> [!TIP]
> Recuerda tener tu servidor y la herramienta docker perfectamente actualizado a la ultima versión disponible.

>[!NOTE]
> Puedes realizar algunos cambio en el documento `docker-compose.yml` para modificar la versión Mongo. Si decides usar una versión más reciente, comprueba la compatibilidad de tu procesador con tecnología `AVX`[^1].

### 2. Configurar el access control

Para configurar el access control de la base de datos mongo se debe crear una keyfile, durante todo este proceso se usará el super usuario, `sudo su`. Lo primero será crear el archivo.
```
touch mongo-keyfile
```
Posterior mente se creará una clave y se intriducirá en el archivo anteriormente creado
```
open rand -base64 756 > mongo-keyfile
```
Y finalmente se dota de los permisos necesarios al archivo
```
chmod 400 mongo-keyfile
```

Los usuarios que podrán acceder a la base de datos se indicarán en el `YAML`dentro del apartado `enviroments` del `mongo-express`.

```
  - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
  - ME_CONFIG_MONGODB_ADMINPASSWORD=password123
```



### 3. Configurar ReplicaSet

Se entrará en uno de los contenedores mongo y se introducirá:
```
docker exec -it mongo1 mongo --eval "rs.initiate({_id: \"myReplicaSet\", members: [{_id: 0, host: \"mongo1\"}, {_id: 1, host:
 \"mongo2\"}, {_id: 2, host: \"mongo3\"}]})"
```
Una vez introducido, la información del contenedor primario se replicará en los demás

### 4. Añadir usuarios

Para añadir usuarios se entrará en el servidor primario y se introducirá el usuario y la contraseña con el comando `db.createUser`.
```
 db.createUser({
...   user: "admin",
...   pwd: "password123",
...   roles: [{ role: "root", db: "admin" }]
... })
```

### 5. Migrar la base de datos

Por ultimo se introducirá la base de datos que se quiera almacenar. En este caso lo haremos en formato `.csv` con el comando `mongoimport`. 
```
 mongoimport --db Ventas --collection Productos --type csv --file Productos.csv --headerline -u admin -p password123 --authen
ticationDatabase admin
```


### 6. web
Para montar el archivo web con el que dar apariencia a tu API, modifica la linea 
```
app.get('/', (req, res) => {
  res.send('Hello World!');
});
```

por la siguiente linea
```
app.get('/', (req, res) => {
  res.sendFile(path.resolve(__dirname, 'public', 'index.html'));
});
```

Y posteriormente, crea un directorio llamado public donde se encontrará el index.html que hará de pagina cuando se conecte al servidor web




Y ya estaría configurado el cluster en el servidor.

> [!NOTE] 
> Este es el Proyecto de Fin de Ciclo de Aitor Ramos y Alejandro Jimeno, alumnos del Ciclo Formativo de Grado Superior de Administración de Sistemas Informáticos en Red

> [!CAUTION]
> Este proyecto está pendiente de calificación

[^1]: En este caso los contenedores Mongo son con la versioón 4.4.29