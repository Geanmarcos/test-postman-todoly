# 🚀 Todo.ly API Test Automation - GT

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

## 🌍 Entorno y Variables

### Variables de Entorno (Todoly_UAT.postman_environment.json)

| Variable | Valor | Tipo | Descripción |
|----------|--------|------|-------------|
| baseUrl | https://todo.ly/api | default | URL base de la API |
| userName | (Secret) | default | Nombre de usuario para autenticación |
| password | (Secret) | default | Contraseña para autenticación |

### Variables de Colección (Todoly.postman_collection.json)

| Variable | Valor Predeterminado | Descripción |
|----------|----------------------|-------------|
| token | (generado dinámicamente) | Token JWT obtenido en login |
| formatType | json | Formato de respuesta |
| projectId | (generado dinámicamente) | ID del proyecto creado |
| projectName | JB API TEST | Nombre del proyecto de prueba |
| icon | 4 | Icono del proyecto |
| newProjectName | JB API TEST V2 | Nombre actualizado del proyecto |
| newIcon | 2 | Icono actualizado |
| itemId | (generado dinámicamente) | ID del ítem creado |
| itemName | JB API TEST - ITEM | Nombre del ítem |
| itemType | 2 | Tipo de ítem |
| newItemName | JB API TEST - ITEM V2 | Nombre actualizado del ítem |
| newItemType | 3 | Tipo actualizado del ítem |
| checkedItem | true | Estado de completado del ítem |
| projectNameTooShort | (vacío) | Para validar límite mínimo de caracteres |
| invalidProjectId | 9999999 | ID inexistente para pruebas negativas |
| itemNameTooShort | (vacío) | Para validar límite mínimo de caracteres en ítems |
| invalidItemId | 99999999 | ID inexistente para pruebas negativas |
| projectNameSInjectBd | '; DROP TABLE PROJECTS;-- | Payload SQL Injection |
| projectNameSInjectXss | (`<script>alert('xss')</script>`) | Payload XSS |

---

# ✅ Cobertura de Pruebas

## 1. Autenticación

| Escenario | Cobertura |
|------------|------------|
| Generación de Token | ✅ |
| Uso de Token para autenticación | ✅ |
| Cierre de sesión | ✅ |
| Validación de acceso sin Token (102) | ✅ |
| Validación de Token inválido | ✅ |
| Validación con Token expirado | ✅ |

---

## 2. Gestión de Proyectos

### Endpoints cubiertos

| Operación | Método HTTP | Estado |
|------------|------------|---------|
| Crear Proyecto | POST | ✅ |
| Actualizar Proyecto | PUT | ✅ |
| Buscar Proyecto | GET | ✅ |
| Eliminar Proyecto | DELETE | ✅ |

### Validaciones - Casos Positivos

✅ Status Code 200

✅ Respuesta JSON válida

✅ Validación de nombre de proyecto

✅ Validación de icono

✅ Persistencia de ID generado dinámicamente

✅ Confirmación de eliminación del recurso

### Validaciones - Casos Negativos

✅ Crear con nombre demasiado corto (305)

✅ Búsqueda de proyecto con ID inválido (402)

✅ Eliminación de proyecto inexistente (301)

---

## 3. Gestión de Ítems

### Endpoints cubiertos

| Operación | Método HTTP | Estado |
|------------|------------|---------|
| Crear Ítem | POST | ✅ |
| Actualizar Ítem | PUT | ✅ |
| Buscar Ítem | GET | ✅ |
| Eliminar Ítem | DELETE | ✅ |

### Validaciones - Casos Positivos

✅ Status Code 200

✅ Respuesta JSON válida

✅ Asociación correcta con el proyecto creado

✅ Validación de nombre actualizado

✅ Validación de estado Checked

✅ Confirmación de eliminación del ítem

### Validaciones - Casos Negativos

✅ Crear con nombre demasiado corto (308)

✅ Eliminación de ítem inexistente (301)

---

## 4. Pruebas de Seguridad

### SQL Injection

✅ Inyección en campo de BD (`'; DROP TABLE PROJECTS;--`)

✅ Inyección XSS (`<script>alert('xss')</script>`)

✅ Payload mixto validando límite de caracteres

---

# 📊 Resumen de Cobertura Funcional

| Módulo | Cobertura |
|----------|-----------|
| Authentication | ✅ 100% |
| Projects CRUD | ✅ 100% |
| Projects Negative | ✅ 100% |
| Items CRUD | ✅ 100% |
| Items Negative | ✅ 100% |
| Security (SQL Injection/XSS) | ✅ 100% |

### Escenarios Positivos Cubiertos

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

### Escenarios Negativos Cubiertos

- Acceso sin autenticación.
- Crear proyecto con contenido inválido.
- Buscar proyecto con ID inexistente.
- Eliminar proyecto que no existe.
- Crear ítem con contenido inválido.
- Eliminar ítem inexistente.
- Pruebas de SQL Injection.
- Pruebas de XSS.

---

## 🔄 Flujo de Ejecución

### Flujo Principal (E2E)

```
┌─────────────────────────────────────┐
│     Login Success                   │
│     ↓                               │
│  open-authentication (get token)    │
└─────────────────────────────────────┘
             ↓
┌─────────────────────────────────────┐
│  Test Happy Path - E2E              │
│                                     │
│  ├─ Project CRUD                   │
│  │  ├─ crear-project               │
│  │  ├─ update-project              │
│  │  └─ search-project              │
│  │                                 │
│  └─ Item CRUD                      │
│     ├─ create-item                 │
│     ├─ update-item                 │
│     └─ search-item                 │
│                                     │
│  └─ Delete Project & Item          │
│     ├─ delete-project              │
│     └─ delete-item                 │
└─────────────────────────────────────┘
             ↓
┌─────────────────────────────────────┐
│  Logout Success                     │
│  ↓                                  │
│  close-authentication (invalidate)  │
└─────────────────────────────────────┘
```

### Estructura de Test Suites

**1. Login Success** - Obtiene token válido

**2. Test Happy Path - E2E** - Flujo completo exitoso
   - Projects: Create, Update, Search
   - Items: Create, Update, Search
   - Delete: Item, Project

**3. Test Project - Negative** - Validaciones de errores en Proyectos
   - Error 305: Create Project (nombre demasiado corto)
   - Error 402: Search Project (ID inválido)
   - Error 301: Delete Project (no existe)

**4. Test Item - Negative** - Validaciones de errores en Ítems
   - Error 308: Create Item (nombre demasiado corto)
   - Error 301: Delete Item (no existe)

**5. Test SQL Injection** - Pruebas de seguridad
   - SQL Injection en BD
   - XSS injection
   - Payload mixto

**6. Test Authentication - Negative** - Validaciones de autenticación
   - Error 102: Create Project sin autenticación
   - Error 102: Create Project sin token
   - Error 102: Create Project con token expirado

**7. Logout Success** - Cierre de sesión

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

# 🚀 Integración Continua - GitHub Actions

El proyecto implementa un pipeline completo de GitHub Actions (`node.js.yml`) que ejecuta automáticamente las pruebas y genera reportes en cada cambio.

## Disparadores del Workflow

El workflow se ejecuta automáticamente en los siguientes eventos:

```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

## Configuración del Entorno

- **Sistema Operativo**: Ubuntu (ubuntu-latest)
- **Node.js**: v24.x
- **Dependencias instaladas**:
  - `newman` - Ejecutor de colecciones Postman
  - `newman-reporter-allure` - Generador de reportes en formato Allure
  - `allure` - CLI v3 para generar reportes visuales

## Pasos del Pipeline

| Paso | Descripción | Comando |
|------|-------------|---------|
| 1. Checkout | Descarga el código del repositorio | `actions/checkout@v7` |
| 2. Setup Node.js | Configura Node.js v24.x | `actions/setup-node@v7` |
| 3. Instalar dependencias | Instala Newman, reporters y Allure CLI | `npm install --save-dev newman newman-reporter-allure allure` |
| 4. Ejecutar tests | Ejecuta la colección Postman con inyección de credenciales desde Secrets | `newman run` |
| 5. Recuperar historial | Obtiene el reporte anterior desde rama gh-pages | `actions/checkout@v7 (ref: gh-pages)` |
| 6. Restaurar historial | Copia `history.jsonl` para mantener persistencia | Copia archivo |
| 7. Crear configuración Allure | Genera `.allurerc.mjs` con configuración de historial | Crea archivo |
| 8. Generar reporte | Genera reporte visual con Allure v3 | `npx allure generate` |
| 9. Publicar en GitHub Pages | Despliega el reporte en rama gh-pages | `peaceiris/actions-gh-pages@v4` |

## Variables de Entorno (Secrets)

Por seguridad, las credenciales se inyectan desde GitHub Secrets:

```yaml
env:
  API_USER: ${{ secrets.API_USER }}
  API_PASSWORD: ${{ secrets.API_PASSWORD }}
```

Luego se pasan a Newman como variables:

```bash
--env-var "userName=$API_USER" \
--env-var "password=$API_PASSWORD"
```

**Nota**: Configura estos secrets en: `Settings → Secrets and variables → Actions`

## Ejecución de Tests en el Workflow

```bash
mkdir -p output/allure-results
npx newman run Todoly.postman_collection.json \
  -e Todoly_UAT.postman_environment.json \
  --env-var "userName=$API_USER" \
  --env-var "password=$API_PASSWORD" \
  --reporters cli,allure \
  --reporter-allure-resultsDir output/allure-results
```

**Parámetros utilizados**:
- `--reporters cli,allure` - Genera salida en CLI y formato Allure
- `--reporter-allure-resultsDir` - Directorio donde se guardan resultados JSON
- `continue-on-error: true` - El workflow continúa incluso si fallan tests

## Gestión del Historial en Allure v3

El workflow mantiene un historial persistente de ejecuciones:

### Configuración de Allure (`.allurerc.mjs`)

```javascript
export default {
  name: "Newman Todoly Tests",
  output: "allure-report",
  historyPath: "./.allure-history/history.jsonl",
  appendHistory: true,
};
```

**Características**:
- `historyPath` - Archivo único JSONL para historial (Allure v3)
- `appendHistory: true` - Agrega nuevos resultados sin perder datos anteriores
- Permite generar gráficos de tendencias a lo largo del tiempo

### Flujo del Historial

1. Recupera `history.jsonl` de la rama `gh-pages` (ejecuciones anteriores)
2. Restaura el archivo en `.allure-history/`
3. Genera nuevo reporte que agrega al historial existente
4. Publica reporte + historial actualizado en `gh-pages`

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

- Data Driven Testing (parametrización de tests).
- Cobertura de endpoints adicionales de Todo.ly (carpetas, filtros, etc.).
- Ejecución programada mediante cron jobs.
- Notificaciones de resultados (Slack, email).
- Integración con herramientas de gestión de pruebas (Jira, TestRail).
- Tests de rendimiento y carga.
- Análisis de cobertura de endpoints.
- Documentación interactiva de API (Swagger/OpenAPI).

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