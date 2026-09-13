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

El proyecto implementa un pipeline de GitHub Actions (`node.js.yml`) que ejecuta automáticamente las pruebas de forma continua.

### Disparadores

El workflow se ejecuta automáticamente en los siguientes eventos:

```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

### Configuración del Entorno

- **SO**: Ubuntu (ubuntu-latest)
- **Node.js**: v24.x
- **Dependencias instaladas**:
  - `newman` - Para ejecutar colecciones Postman
  - `newman-reporter-allure` - Reporter para generar resultados en formato Allure
  - `allure` - Para generar reportes visuales v3

### Proceso de Ejecución

1. **Checkout** - Descarga el código del repositorio
2. **Setup Node.js** - Configura Node.js v24.x
3. **Instalación de dependencias** - Instala Newman, reporter de Allure y CLI de Allure
4. **Ejecución de tests** - Ejecuta la colección Postman con variables de entorno inyectadas desde Secrets
5. **Generación de resultados Allure** - Los resultados se guardan en `output/allure-results`
6. **Recuperación del historial** - Obtiene el reporte anterior desde `gh-pages` para mantener el historial
7. **Generación del reporte** - Genera el reporte visual con Allure v3
8. **Publicación en GitHub Pages** - Despliega el reporte en `gh-pages` para acceso permanente

---

## 📊 Reportes y Resultados

### Allure Report v3

El workflow genera reportes avanzados usando **Allure v3** con las siguientes características:

- **Historial persistente** - Mantiene un registro histórico de todas las ejecuciones mediante `history.jsonl`
- **Reporte visual interactivo** - Acceso a través de GitHub Pages
- **Métricas detalladas** - Gráficos de tendencias, distribución de resultados y cobertura
- **Trazabilidad completa** - Asociación entre escenarios, pasos y resultados

### Acceso al reporte

Una vez desplegado, el reporte es accesible en:

```
https://<username>.github.io/<repository>/
```

### Configuración de Allure

El workflow utiliza la siguiente configuración (`.allurerc.mjs`):

```javascript
export default {
  name: "Newman Todoly Tests",
  output: "allure-report",
  historyPath: "./.allure-history/history.jsonl",
  appendHistory: true,
};
```

Esto permite:
- Agregar nuevos resultados al historial sin perder datos anteriores
- Generar gráficos de tendencias a lo largo del tiempo
- Mantener la coherencia del reporte

---

## 📈 Buenas Prácticas Aplicadas

- Gestión segura de credenciales mediante GitHub Secrets.
- Uso de variables dinámicas.
- Encadenamiento de datos entre requests.
- Validaciones funcionales automatizadas.
- Integración Continua (CI) con GitHub Actions.
- Generación de reportes visuales Allure v3.
- Historial persistente de ejecuciones.
- Separación de configuración por entorno.
- Publicación automática de reportes en GitHub Pages.

---

## 🚧 Mejoras Futuras

- Casos negativos (401, 403, 404, 500).
- Validaciones de seguridad.
- Data Driven Testing.
- Cobertura de endpoints adicionales de Todo.ly.
- Ejecución programada mediante cron jobs.
- Notificaciones de resultados (Slack, email).
- Integración con herramientas de gestión de pruebas.

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