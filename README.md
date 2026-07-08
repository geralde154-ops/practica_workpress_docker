# Practica WordPress con Docker Compose y Jenkins

Gerald Elizondo Mora



Este proyecto implementa un ambiente completo de WordPress utilizando Docker Compose,
versionado con Git y desplegado automáticamente mediante un pipeline de Jenkins.

## Estructura del proyecto

```
.
├── docker-compose.yml   # Definición de los servicios WordPress y MySQL
├── .env                 # Variables de entorno (credenciales, puertos, nombres)
├── Jenkinsfile           # Pipeline de despliegue automático
└── README.md             # Documentación del proyecto
```

## Requisitos previos

* Docker y Docker Compose instalados.
* Jenkins instalado y con el plugin de Docker Pipeline habilitado.
* El usuario de Jenkins debe tener permisos para ejecutar comandos Docker.
* Cuenta de GitHub con el repositorio del proyecto.

## Actividad 1 y 2 — Docker Compose y configuración

El archivo `docker-compose.yml` define dos servicios:

* **db**: contenedor de MySQL 8.0, con volumen persistente `db\\\_data` y variables
de entorno tomadas desde `.env` (usuario, contraseña, nombre de la base de datos).
* **wordpress**: contenedor de WordPress, conectado a `db` mediante la red
personalizada `wordpress\\\_net`, con volumen persistente `wp\\\_data` y expuesto en
el puerto definido por `WORDPRESS\\\_PORT` (por defecto 8080).

Ambos servicios tienen política de reinicio `restart: always`.

## Actividad 3 — Levantar el ambiente

```bash
docker compose up -d
docker ps
docker network ls
docker volume ls
```

Se debe verificar que:

* Los contenedores `mysql\\\_db` y `wordpress\\\_app` estén en estado `Up`.
* La red `wordpress\\\_net` aparezca listada.
* Los volúmenes `db\\\_data` y `wp\\\_data` existan.

## Actividad 4 — Validar comunicación entre contenedores

```bash
docker inspect mysql\\\_db
docker inspect wordpress\\\_app
```

En la sección `NetworkSettings.Networks` de ambos contenedores debe aparecer
`wordpress\\\_net`, confirmando que pertenecen a la misma red y pueden comunicarse.

También se puede validar la conexión directamente desde el contenedor de WordPress:

```bash
docker exec -it wordpress\\\_app bash
apt-get update \\\&\\\& apt-get install -y default-mysql-client
mysql -h db -u wpuser -p
```

## Actividad 5 — Control de versiones con Git

```bash
git init
git add .
git commit -m "Estructura inicial del proyecto WordPress con Docker Compose"
git remote add origin https://github.com/geralde154-ops/practica\\\_workpress\\\_docker.git
git branch -M main
git push -u origin main
```

Se deben realizar al menos tres commits descriptivos a lo largo del desarrollo
(configuración inicial, agregado del pipeline, ajustes de documentación, etc.).

## Actividad 6 — Pipeline de Jenkins

El `Jenkinsfile` define las siguientes etapas:

1. **Checkout**: obtiene el código desde el repositorio Git.
2. **Detener ambiente anterior**: ejecuta `docker compose down`.
3. **Desplegar ambiente**: ejecuta `docker compose up -d`.
4. **Verificar despliegue**: ejecuta `docker ps`, `docker network ls` y `docker volume ls`.

Para configurarlo en Jenkins:

1. Crear un nuevo item de tipo **Pipeline**.
2. En la sección **Pipeline**, seleccionar **Pipeline script from SCM**.
3. Configurar el repositorio Git del proyecto y la rama correspondiente.
4. Indicar que el script a usar es el `Jenkinsfile` ubicado en la raíz del repositorio.
5. Ejecutar el job (**Build Now**) y revisar la consola de salida.

## Actividad 7 — Validación final

Acceder desde el navegador a:

```
http://localhost:8080
```

Debe visualizarse el asistente de instalación de WordPress (selección de idioma
y formulario de configuración del sitio), confirmando que el ambiente quedó
funcional.

## Entregables

* Repositorio en GitHub con el proyecto completo.
* `docker-compose.yml`, `.env`, `Jenkinsfile`.

