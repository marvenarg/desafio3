# Desafío 3 del Modulo 4 - Bootcamp Devops Engineer <br><br> 

## Jenkins Pipeline + Docker

Este repositorio contiene el `Jenkinsfile` para automatizar el despliegue de una aplicación PHP en Docker, incluyendo construcción de imagen, ejecución de contenedor, validación y publicación en Docker Hub.

## 📋 Requisitos previos

- Jenkins instalado
- Docker instalado y accesible para Jenkins
- Credenciales de Docker Hub configuradas en Jenkins
- Plugins necesarios instalados (ver `REPLICACION.md`)

## ▶️ ¿Qué hace el pipeline?

1. Clona la app desde https://github.com/marvenarg/desafio2
2. Construye una imagen Docker desde `webapp/Dockerfile`
3. Elimina cualquier contenedor anterior con el mismo nombre
4. Crea y ejecuta un nuevo contenedor
5. Verifica la existencia del archivo `/var/www/html/index.php`
6. Si pasa, publica la imagen en Docker Hub con los tags `vX` y `latest`
7. Elimina el contenedor de prueba y hace logout de Docker

## 🧪 Ejecución

1. Crear un pipeline en Jenkins con:
   - Tipo: "Pipeline script from SCM"
   - URL del repositorio: `https://github.com/<TU_USUARIO>/desafio2-jenkins.git`
   - Branch: `main` 

2. Ejecutar el pipeline

## 🔄 Automatización (opcional)

Dentro del `Jenkinsfile` hay una sección comentada con triggers:

```groovy
// triggers {
//     githubPush()
//     cron('H/15 * * * *')
// }
```
Podés activarla si querés que el pipeline corra automáticamente por cambios en GitHub o cada cierto tiempo.

