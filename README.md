# test-postman-todoly

Pruebas automatizadas de la API **Todo.ly** usando Postman y Newman.

## 🚀 Stack

- [Postman](https://www.postman.com/)  
- [Newman](https://www.postman.com/api-first/)  
- GitHub Actions (CI/CD)

## 📂 Project Structure

- `todoly.postman_collection.json` → colección con requests y tests automatizados.  
- `.github/workflows/newman.yml` → pipeline para ejecutar Newman en cada push/PR.  
- `newman/` → carpeta de reportes generados (ignorada en git).  

## 🔑 Variables usadas en la colección

La colección define variables para parametrizar las requests:

| Variable            | Descripción                          | Ejemplo             |
|---------------------|--------------------------------------|---------------------|
| `PROJECT_ID`        | ID del proyecto creado dinámicamente | Se setea en runtime |
| `TYPE_FORMAT`       | Formato de respuesta                 | `json`              |
| `PROJECT_NAME`      | Nombre inicial del proyecto          | `JB API TEST`       |
| `ICON`              | Ícono inicial del proyecto           | `4`                 |
| `PROJECT_NAME_NEW`  | Nombre actualizado del proyecto      | `JB API TEST V2`    |
| `ICON_NEW`          | Ícono actualizado del proyecto       | `2`                 |

## 🧪 Requests incluidas

1. **crear-project** → `POST /projects.json`  
   - Crea un proyecto nuevo y guarda el `PROJECT_ID`.  
   - Valida status 200, nombre y ícono.  

2. **update-project** → `PUT /projects/{PROJECT_ID}.json`  
   - Actualiza nombre e ícono del proyecto.  
   - Valida status 200 y valores actualizados.  

3. **search-project** → `GET /projects/{PROJECT_ID}.json`  
   - Busca el proyecto por ID.  
   - Valida status 200 y datos correctos.  

4. **delete-project** → `DELETE /projects/{PROJECT_ID}.json`  
   - Elimina el proyecto.  
   - Valida status 200 y campo `Deleted = true`.  

## ⚙️ Setup local

1. Instala Newman:
   ```bash
   npm install -g newman
   ```
2. (Opcional) Instala el reporter HTML:
   ```bash
   npm install -g newman-reporter-html
   ```

## ▶️ Ejecutar pruebas

```bash
newman run todoly.postman_collection.json -r cli,html --reporter-html-export newman/report.html
```

## 🔄 CI/CD con GitHub Actions

Este proyecto incluye un workflow de GitHub Actions (`.github/workflows/node.js.yml`) que ejecuta automáticamente la colección de Postman en cada push o pull request.  

El reporte HTML se guarda como artefacto descargable en la sección *Actions*.

![CI/CD](https://github.com/Geanmarcos/test-postman-todoly/actions/workflows/node.js.yml)


## 📖 Documentación API

- [Sitio oficial Todo.ly](https://todo.ly/)  
- [Wiki de la API](https://todo.ly/apiwiki/?projects/todo-ly-rest-api-method-post-projects)  

---