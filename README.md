# 🚀 Todo.ly API Test Automation

Automatización de pruebas para la API REST de Todo.ly utilizando Postman, Newman y GitHub Actions.

El proyecto implementa pruebas funcionales automatizadas para validar los principales flujos de negocio de la aplicación, incluyendo autenticación, gestión de proyectos y gestión de ítems, ejecutándose de forma automática mediante Integración Continua (CI).

---

## 📋 Objetivo

Validar los principales servicios de la API de Todo.ly mediante pruebas automatizadas que permitan:

- Verificar el funcionamiento de los endpoints.
- Garantizar la integridad de los datos.
- Detectar regresiones de manera temprana.
- Ejecutar pruebas automáticamente en cada Push o Pull Request.

---

## 🛠️ Tecnologías Utilizadas

- Postman
- Newman
- JavaScript (Tests de Postman)
- GitHub Actions
- REST APIs

---

## 📂 Estructura del Proyecto

```text
.
├── Todoly.postman_collection.json
├── Todoly_UAT.postman_environment.json
├── README.md
└── .github
    └── workflows
        └── node.js.yml
```

---

## 🔐 Gestión de Credenciales

Por seguridad las credenciales no se almacenan dentro del repositorio.

GitHub Actions utiliza Secrets para inyectar la información sensible durante la ejecución:

| Secret | Descripción |
|----------|-------------|
| API_USER | Usuario de Todo.ly |
| API_PASSWORD | Contraseña de Todo.ly |

Variables utilizadas dentro del workflow:

```yaml
env:
  API_USER: ${{ secrets.API_USER }}
  API_PASSWORD: ${{ secrets.API_PASSWORD }}
```

---

## 🌍 Entorno

El proyecto utiliza el siguiente entorno:

| Variable | Valor |
|-----------|--------|
| baseUrl | https://todo.ly/api |
| formatType | json |

---

# ✅ Cobertura de Pruebas

## Autenticación

| Escenario | Cobertura |
|------------|------------|
| Generación de Token | ✅ |
| Uso de Token para autenticación | ✅ |
| Cierre de sesión | ✅ |

---

## Gestión de Proyectos

### Endpoints cubiertos

| Operación | Método HTTP | Estado |
|------------|------------|---------|
| Crear Proyecto | POST | ✅ |
| Actualizar Proyecto | PUT | ✅ |
| Buscar Proyecto | GET | ✅ |
| Eliminar Proyecto | DELETE | ✅ |

### Validaciones implementadas

✅ Status Code 200

✅ Respuesta JSON válida

✅ Validación de nombre de proyecto

✅ Validación de icono

✅ Persistencia de ID generado dinámicamente

✅ Confirmación de eliminación del recurso

---

## Gestión de Ítems

### Endpoints cubiertos

| Operación | Método HTTP | Estado |
|------------|------------|---------|
| Crear Ítem | POST | ✅ |
| Actualizar Ítem | PUT | ✅ |
| Buscar Ítem | GET | ✅ |
| Eliminar Ítem | DELETE | ✅ |

### Validaciones implementadas

✅ Status Code 200

✅ Respuesta JSON válida

✅ Asociación correcta con el proyecto creado

✅ Validación de nombre actualizado

✅ Validación de estado Checked

✅ Confirmación de eliminación del ítem

---

# 📊 Resumen de Cobertura Funcional

| Módulo | Cobertura |
|----------|-----------|
| Authentication | ✅ 100% |
| Projects CRUD | ✅ 100% |
| Items CRUD | ✅ 100% |

### Escenarios Cubiertos

- Login mediante Token.
- Creación de Proyecto.
- Actualización de Proyecto.
- Consulta de Proyecto.
- Eliminación de Proyecto.
- Creación de Ítem.
- Actualización de Ítem.
- Consulta de Ítem.
- Eliminación de Ítem.
- Logout.

---

## 🔄 Flujo de Ejecución

```text
Open Authentication
        ↓
Create Project
        ↓
Update Project
        ↓
Search Project
        ↓
Create Item
        ↓
Update Item
        ↓
Search Item
        ↓
Delete Item
        ↓
Delete Project
        ↓
Close Authentication
```

---

## ⚙️ Ejecución Local

### Instalar Newman

```bash
npm install -g newman
```

### Instalar Reporter HTML

```bash
npm install -g newman-reporter-html
```

### Ejecutar colección

```bash
newman run Todoly.postman_collection.json \
-e Todoly_UAT.postman_environment.json \
--env-var "userName=USUARIO" \
--env-var "password=PASSWORD" \
-r cli,html \
--reporter-html-export newman/report.html
```

---

# 🚀 Integración Continua

El proyecto cuenta con un pipeline de GitHub Actions que ejecuta automáticamente las pruebas:

### Disparadores

```yaml
on:
  push:
    branches: [main]

  pull_request:
    branches: [main]
```

### Flujo CI

1. Checkout del repositorio.
2. Configuración de NodeJS.
3. Instalación de Newman.
4. Instalación del Reporter HTML.
5. Ejecución de la colección Postman.
6. Generación del reporte HTML.
7. Publicación del reporte como artefacto.

---

## 📄 Reportes

Después de cada ejecución:

- Se genera un reporte HTML.
- El reporte queda disponible como artefacto dentro de GitHub Actions.
- Permite revisar resultados, tiempos de ejecución y validaciones ejecutadas.

---

## 📈 Buenas Prácticas Aplicadas

- Gestión segura de credenciales mediante GitHub Secrets.
- Uso de variables dinámicas.
- Encadenamiento de datos entre requests.
- Validaciones funcionales automatizadas.
- Integración Continua (CI).
- Generación de evidencias de ejecución.
- Separación de configuración por entorno.

---

## 🚧 Mejoras Futuras

- Casos negativos (401, 403, 404, 500).
- Validaciones de seguridad.
- Data Driven Testing.
- Integración con Allure Reports.
- Cobertura de endpoints adicionales de Todo.ly.
- Ejecución programada mediante cron jobs.
- Métricas de calidad y tendencias de ejecución.

---

## 📚 Referencias

- https://todo.ly/
- https://todo.ly/apiwiki/
- https://www.postman.com/
- https://www.npmjs.com/package/newman
- https://github.com/features/actions

---

## 👨‍💻 Autor

**Geanmarcos Antonio Tataje Tipacti**

QA Automation Engineer

Proyecto desarrollado como práctica de automatización de APIs utilizando Postman + Newman + GitHub Actions.