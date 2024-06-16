# ProyectoFinCiclo
> [!NOTE] 
> Este es el Proyecto de Fin de Ciclo de Aitor Ramos y Alejandro Jimeno, alumnos del Ciclo Formativo de Grado Superior de Administración de Sistemas Informáticos en Red

> [!CAUTION]
> Este proyecto está pendiente de calificación

## Despliegue y configuración de Mongo con interfaz web con Docker

### 1. Descarga

Descarga el documento comprimido del cluster 

<!--Enlace para descargar el archivo o no-->

> [!TIP]
> Recuerda tener tu servidor y la herramienta docker perfectamente actualizado a la ultima versión disponible.

>[!NOTE]
> Puedes realizar algunos cambio en el documento *docker-compose.yml* para modificar la versión Mongo. Si decides usar una versión más reciente, comprueba la compatibilidad de tu procesador con tecnología AVX[^1].

### 2. Configurar el access control

En el documento YAML se debe configurar los usuarios para el access control:

<!--Info sobre linea, etc-->

### 3. Web


### 4. Iniciar el YAML
Una vez se tenga configurado los documentos del YAML se iniciará el documento.

```
    docker-compose up -d
```

### 5. Comprobaciones

Para comprobar solo deber


[^1]: En nuestro despliegue se hace uso de Mongo v4.4.29