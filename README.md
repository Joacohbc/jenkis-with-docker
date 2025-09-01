# Jenkins con Docker CLI

Este `Dockerfile` personaliza la imagen oficial de Jenkins LTS (`jenkins/jenkins:lts`) para incluir el **CLI de Docker**. Esto permite que los trabajos y pipelines de Jenkins puedan ejecutar comandos de Docker, interactuando directamente con el daemon del host.

Es ideal para flujos de CI/CD que necesitan construir, probar y desplegar imágenes de Docker.

## Características

  * **Imagen Base:** `jenkins/jenkins:lts`.
  * **Herramientas:** Instala el **Docker CE CLI** (`docker-ce-cli`).
  * **Permisos:**
      * Agrega el usuario `jenkins` al grupo `docker`.
      * Configura los permisos del socket de Docker (`/var/run/docker.sock`) usando `setfacl` para permitir el acceso al grupo `docker`.
  * **Plugins preinstalados:**
      * `blueocean`: Una interfaz moderna para pipelines.
      * `docker-workflow`: Para integrar comandos de Docker en los pipelines.
      * `json-path-api` y `locale`: Dependencias comunes.

## ¿Cómo usarlo?

### 1\. Construir la imagen

```bash
docker build -t mi-jenkins-docker .
```

### 2\. Ejecutar el contenedor

Para que el contenedor de Jenkins pueda controlar el Docker del host, es crucial montar el socket de Docker usando un volumen.

```bash
docker run -p 8080:8080 -p 50000:50000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v jenkins_home:/var/jenkins_home \
  mi-jenkins-docker
```
