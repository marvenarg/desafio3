
---

### ✅ REPLICACION.md

```markdown
# REPLICACIÓN DEL PIPELINE EN UNA NUEVA INSTANCIA DE JENKINS

Este documento detalla los pasos para configurar desde cero una instancia de Jenkins que pueda ejecutar este pipeline correctamente.

---

## 1. ✅ Plugins necesarios en Jenkins

Instalar los siguientes plugins desde "Administrar Jenkins" → "Plugins":

- **Docker Pipeline** (`docker-workflow`)
- **Git plugin** (`git`)
- **Pipeline** (`workflow-aggregator`)
- **Credentials Binding** (`credentials-binding`)
- **GitHub plugin** (opcional si usás webhooks)
- **Docker Commons**

---

## 2. ✅ Herramientas instaladas en el servidor de Jenkins

- **Docker Engine**
  - Jenkins debe poder usar Docker (usualmente agregando el usuario `jenkins` al grupo `docker`):
    ```bash
    sudo usermod -aG docker jenkins
    sudo systemctl restart jenkins
    ```

- **Git**
  - Necesario para clonar repositorios
  - Verificá con `git --version`

---

## 3. ✅ Credenciales en Jenkins

Ir a **"Administrar Jenkins" → "Credentials" → (global)** y crear una credencial:

- Tipo: **Username with password**
- ID: `docker-hub-credentials` ✅ (este ID debe coincidir con el usado en el Jenkinsfile)
- Username: tu usuario de Docker Hub
- Password: ⚠️ Usar un **Access Token** o Personal access tokens (PAT), no tu contraseña real

---

## 4. ✅ Configurar el pipeline

En Jenkins:

1. Crear un **nuevo item** → tipo: *Pipeline*
2. En la configuración:
   - Marcar: "Pipeline script from SCM"
   - SCM: Git
   - URL: `https://github.com/<TU_USUARIO>/desafio2-jenkins.git`
   - Branch: `main`
3. Guardar y ejecutar

---

## 5. 🔁 Webhook (opcional)

Para que el pipeline se ejecute automáticamente al hacer push en GitHub:

1. En GitHub, ir a:
   `Settings > Webhooks > Add webhook`
2. Payload URL: `http://<tu-servidor>:8080/github-webhook/`
3. Content type: `application/json`
4. Events: "Just the push event"
5. Asegurate de que Jenkins tenga configurado el plugin GitHub y GitHub Webhook

---

Con estos pasos, el pipeline debería poder replicarse completamente en otra instancia Jenkins.


