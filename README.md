# 📚 Vektor AI - Documentación

Documentación técnica completa del proyecto Vektor AI.

**Hackathon:** Oracle ONE + Alura LATAM - NoCountry  
**Equipo:** n° 71  
**Versión:** 2.0.0

---

## 🎯 Descripción

Este repositorio contiene toda la documentación técnica del proyecto Vektor AI:
- Contratos de integración
- Diagramas de arquitectura
- Guías de desarrollo
- Datasets y recursos compartidos
- Manuales de usuario

---

## 📁 Estructura del Repositorio

```
vektor-ai-docs/
├── contracts/
│   ├── CONTRATO_INTEGRACION.md
│   └── API_CHANGELOG.md
├── architecture/
│   ├── system-diagram.md
│   ├── data-flow.md
│   └── deployment.md
├── datasets/
│   ├── aerolineas.json
│   ├── airports.json
│   └── README.md
├── guides/
│   ├── setup-guide.md
│   ├── deployment-guide.md
│   └── troubleshooting.md
├── api-examples/
│   ├── postman-collection.json
│   ├── curl-examples.md
│   └── javascript-examples.md
├── user-manual/
│   ├── es/
│   │   └── manual-usuario.md
│   └── en/
│       └── user-manual.md
└── README.md
```

---

## 📄 Documentos Principales

### 1. Contrato de Integración

**Archivo:** `contracts/CONTRATO_INTEGRACION.md`

Documento técnico completo que define:
- Endpoints del sistema (Backend y ML Service)
- Tipos de datos y validaciones
- Estructura de request/response
- Códigos de estado HTTP
- Especificación del modelo ML (15 features)
- Transformaciones y encoders
- Ejemplos de integración
- Reglas de negocio

**Versión Actual:** 2.0.0  
**Última Actualización:** 2026-01-08

### 2. Arquitectura del Sistema

**Archivo:** `architecture/system-diagram.md`

Describe la arquitectura completa:

```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│  Frontend   │─────▶│   REST API   │─────▶│  ML Service │
│   (React)   │      │ (Spring Boot)│      │  (FastAPI)  │
│   Port 5173 │      │   Port 8080  │      │  Port 8001  │
└─────────────┘      └──────┬───────┘      └──────┬──────┘
                            │                      │
                            ▼                      ▼
                     ┌─────────────┐      ┌──────────────┐
                     │ PostgreSQL  │      │OpenWeatherMap│
                     │  Database   │      │     API      │
                     └─────────────┘      └──────────────┘
```

### 3. Dataset: aerolineas.json

**Archivo:** `datasets/aerolineas.json`

Diccionario de aerolíneas con sus aeropuertos de origen y destino:

```json
{
  "9E": {
    "ORIGIN": ["ABE", "ABY", "AEX", ...],
    "DEST": ["ABE", "ABY", "AEX", ...]
  },
  "AA": {
    "ORIGIN": ["ABQ", "AGS", "ALB", ...],
    "DEST": ["ABQ", "AGS", "ALB", ...]
  }
}
```

**Total de Aerolíneas:** 15  
**Total de Aeropuertos:** 397

### 4. Dataset: airports.json

**Archivo:** `datasets/airports.json`

Diccionario completo de aeropuertos con coordenadas:

```json
{
  "ATL": {
    "nombre": "Hartsfield-Jackson Atlanta International Airport",
    "ciudad": "Atlanta",
    "lat": 33.6407,
    "lon": -84.4277
  }
}
```

---

## 🚀 Guías de Desarrollo

### Setup Guide

**Archivo:** `guides/setup-guide.md`

Instrucciones paso a paso para configurar el entorno de desarrollo:

1. Prerrequisitos (Java, Python, Node.js)
2. Clonar repositorios
3. Configurar variables de entorno
4. Instalar dependencias
5. Ejecutar servicios localmente
6. Verificar integración

### Deployment Guide

**Archivo:** `guides/deployment-guide.md`

Guía para desplegar en producción:

1. Preparación del servidor (OCI)
2. Configuración de base de datos
3. Build de aplicaciones
4. Deploy con Docker Compose
5. Configuración de Nginx
6. SSL/HTTPS con Let's Encrypt
7. Monitoreo y logs

### Troubleshooting

**Archivo:** `guides/troubleshooting.md`

Solución a problemas comunes:

- Backend no conecta con ML Service
- Modelo ML no carga
- Errores de validación
- Timeouts en predicciones
- Problemas de CORS
- Base de datos no conecta

---

## 📊 Diagramas

### Flujo de Datos

**Archivo:** `architecture/data-flow.md`

```
Usuario
  │
  │ POST /api/predict
  ▼
┌─────────────────────────────────────┐
│ Backend (Validación)                │
│ 1. Bean Validation                  │
│ 2. Validación de negocio            │
│ 3. Normalización                    │
└────────────┬────────────────────────┘
             │
             │ POST /predict_internal
             ▼
┌─────────────────────────────────────┐
│ ML Service                          │
│ 1. Validar aeropuertos              │
│ 2. Calcular distancia               │
│ 3. Consultar clima                  │
│ 4. Preparar features (15)           │
│ 5. Aplicar encoders                 │
│ 6. Predecir                         │
└────────────┬────────────────────────┘
             │
             │ PredictionResponseDTO
             ▼
┌─────────────────────────────────────┐
│ Backend (Enriquecimiento)           │
│ 1. Agregar metadata                 │
│ 2. Guardar en BD                    │
└────────────┬────────────────────────┘
             │
             ▼
          Usuario
```

---

## 🔧 Ejemplos de API

### Postman Collection

**Archivo:** `api-examples/postman-collection.json`

Colección completa de Postman con:
- Todos los endpoints
- Variables de entorno
- Tests automáticos
- Ejemplos de respuestas

**Importar en Postman:**
1. File → Import
2. Seleccionar `postman-collection.json`
3. Configurar variables de entorno

### cURL Examples

**Archivo:** `api-examples/curl-examples.md`

```bash
# Predicción básica
curl -X POST http://localhost:8080/api/predict \
  -H "Content-Type: application/json" \
  -d '{
    "aerolinea": "DL",
    "origen": "ATL",
    "destino": "JFK",
    "fecha_partida": "2026-01-15T14:30:00"
  }'

# Health check
curl http://localhost:8080/api/health

# Listar aeropuertos
curl http://localhost:8001/airports
```

### JavaScript Examples

**Archivo:** `api-examples/javascript-examples.md`

Ejemplos con Fetch API, Axios, y Node.js.

---

## 📖 Manual de Usuario

### Español

**Archivo:** `user-manual/es/manual-usuario.md`

Manual completo en español que incluye:
- Introducción al sistema
- Cómo hacer una predicción
- Interpretar resultados
- Cambiar idioma y unidades
- Preguntas frecuentes
- Soporte

### English

**Archivo:** `user-manual/en/user-manual.md`

Manual completo en inglés.

---

## 🔄 Changelog

### Versión 2.0.0 (2026-01-08)

**BREAKING CHANGES:**
- Clarificación de formato de `aerolinea` (String IATA, no numérico)
- Especificación completa de las 15 features del modelo
- Documentación detallada de transformaciones

**Nuevas Features:**
- Campo `clima_destino` en respuestas
- Campos `tiempo_respuesta_ms` y `tiempo_cliente_ms`
- Documentación de conversiones de unidades

**Mejoras:**
- Documentación técnica completa del modelo ML
- Especificación de encoders y transformaciones
- Diagramas de flujo detallados

### Versión 1.0.0 (2025-12-25)

- ✨ Versión inicial del proyecto
- 📚 Documentación base
- 📊 Diagramas de arquitectura
- 📄 Contrato de integración v1

---

## 🎯 Recursos Compartidos

### Aerolíneas Soportadas

| Código | Nombre |
|--------|--------|
| 9E | Endeavor Air |
| AA | American Airlines |
| AS | Alaska Airlines |
| B6 | JetBlue Airways |
| DL | Delta Air Lines |
| F9 | Frontier Airlines |
| G4 | Allegiant Air |
| HA | Hawaiian Airlines |
| MQ | Envoy Air |
| NK | Spirit Airlines |
| OH | PSA Airlines |
| OO | SkyWest Airlines |
| UA | United Airlines |
| WN | Southwest Airlines |
| YX | Republic Airways |

### Aeropuertos Principales

Total: 397 aeropuertos en Estados Unidos

**Top 10 por tráfico:**
1. ATL - Atlanta
2. DFW - Dallas/Fort Worth
3. DEN - Denver
4. ORD - Chicago O'Hare
5. LAX - Los Angeles
6. JFK - New York JFK
7. LAS - Las Vegas
8. MCO - Orlando
9. MIA - Miami
10. PHX - Phoenix

---

## 🔍 Búsqueda Rápida

### Encontrar información sobre:

**Endpoints:**
- Ver `contracts/CONTRATO_INTEGRACION.md` Sección 2

**Validaciones:**
- Ver `contracts/CONTRATO_INTEGRACION.md` Sección 3-4

**Modelo ML:**
- Ver `contracts/CONTRATO_INTEGRACION.md` Sección 11

**Despliegue:**
- Ver `guides/deployment-guide.md`

**Problemas comunes:**
- Ver `guides/troubleshooting.md`

---

## 👥 Mantenedores

**Product Owner:**
- Kevin - [@tu-usuario]

**Documentación:**
- Equipo completo contribuye
- Product Owner coordina actualizaciones

---

## 🤝 Cómo Contribuir

### Actualizar Documentación

1. Fork el repositorio
2. Crea una rama: `git checkout -b docs/actualizacion`
3. Edita los archivos Markdown
4. Commit: `git commit -m 'Docs: actualización de X'`
5. Push: `git push origin docs/actualizacion`
6. Abre un Pull Request

### Convenciones

- Usa Markdown estándar
- Incluye ejemplos de código
- Mantén estructura existente
- Actualiza el changelog

---

## 📞 Soporte

**Issues:** https://github.com/Vektor-AI/vektor-ai-docs/issues  
**Discussions:** https://github.com/Vektor-AI/vektor-ai-docs/discussions  
**Email del Equipo:** [email del equipo]

---

## 📄 Licencia

MIT License - ver [LICENSE](LICENSE)

---

## 🏆 Hackathon Oracle ONE 2025

Documentación desarrollada como parte del hackathon final del Programa Oracle Next Education (ONE).

---

## 📚 Enlaces Relacionados

- **Organización GitHub:** https://github.com/Vektor-AI
- **Backend API:** https://github.com/Vektor-AI/vektor-ai-api
- **ML Service:** https://github.com/Vektor-AI/vektor-ai-ml

---

**Documentado con ❤️ por el Equipo 8 - Oracle ONE Hackathon 2025**
