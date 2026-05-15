# 🧠 Motor de Inferencia Lógica como Servicio

Un servicio web que expone un motor de inferencia simbólica basado en **Tau Prolog** a través de una **API REST** construida con **Node.js + Express**, integrando programación **lógica**, **funcional** y **asíncrona**.

---

## 📦 Instalación local (desde código fuente)

### Requisitos previos
- [Node.js](https://nodejs.org/) **v18 o superior**
- npm (incluido con Node.js)

### Pasos

```bash
# 1. Clonar el repositorio
git clone <URL-DEL-REPOSITORIO>
cd motor-inferencia-logica

# 2. Instalar dependencias
npm install

# 3. Iniciar el servidor
npm start
```

El servidor quedará disponible en `http://localhost:3000`.

Para desarrollo con recarga automática:
```bash
npm run dev
```

---

## 🚀 Instalación del ejecutable (recomendado)

El proyecto incluye un ejecutable autónomo compilado con **pkg** que **no requiere tener Node.js instalado**.

### Descargar el ejecutable

Descarga el archivo correspondiente a tu sistema operativo desde la sección **Releases** del repositorio:

| Sistema | Archivo |
|---------|---------|
| Windows x64 | `inference-engine-win.exe` |
| macOS x64   | `inference-engine-macos`  |
| Linux x64   | `inference-engine-linux`  |

### Ejecutar en Windows

```cmd
inference-engine-win.exe
```

### Ejecutar en macOS / Linux

```bash
# Dar permisos de ejecución (solo la primera vez)
chmod +x inference-engine-macos   # o inference-engine-linux

# Ejecutar
./inference-engine-macos          # o ./inference-engine-linux
```

> El ejecutable incluye Node.js y todas las dependencias. Solo necesitas el binario y la carpeta `knowledge/` en el mismo directorio.

---

## 📡 Uso de la API

### Endpoints disponibles

| Método | Ruta        | Descripción                          |
|--------|-------------|--------------------------------------|
| POST   | `/query`    | Ejecutar una consulta Prolog         |
| GET    | `/health`   | Verificar estado del servicio        |
| GET    | `/examples` | Obtener lista de consultas de ejemplo |

---

### POST `/query`

**Body JSON:**
```json
{
  "query": "penalty_applicable(X).",
  "maxSolutions": 10
}
```

| Campo          | Tipo   | Requerido | Descripción                              |
|----------------|--------|-----------|------------------------------------------|
| `query`        | string | ✅ Sí      | Consulta Prolog válida                   |
| `maxSolutions` | number | ❌ No      | Límite de soluciones (default: 50)       |

**Respuesta exitosa:**
```json
{
  "success": true,
  "query": "penalty_applicable(X).",
  "solutionsCount": 3,
  "truncated": false,
  "solutions": [
    { "X": "contract1" },
    { "X": "contract3" },
    { "X": "contract4" }
  ],
  "elapsedMs": 12
}
```

---

### Ejemplos de consultas con `curl`

```bash
# Contratos con penalización aplicable
curl -X POST http://localhost:3000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "penalty_applicable(X)."}'

# ¿Es contract2 exitoso?
curl -X POST http://localhost:3000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "successful_contract(contract2)."}'

# Clientes confiables
curl -X POST http://localhost:3000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "reliable_client(X)."}'

# Resumen completo de contract3
curl -X POST http://localhost:3000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "contract_summary(contract3,C,T,D,V,P,S,H)."}'

# Contratos que requieren auditoría
curl -X POST http://localhost:3000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "audit_required(X)."}'
```

---

## 🗂️ Estructura del proyecto

```
motor-inferencia-logica/
├── src/
│   └── server.js          # Servidor Express + lógica asíncrona/funcional
├── knowledge/
│   └── base.pl            # Base de conocimiento Prolog (hechos + reglas)
├── package.json
└── README.md
```

---

## 🔬 Paradigmas implementados

| Paradigma            | Implementación                                        |
|----------------------|-------------------------------------------------------|
| **Lógico**           | Tau Prolog: hechos, reglas e inferencia automática   |
| **Funcional**        | `pipe`, `normalizeQuery`, `formatSolution` — funciones puras |
| **Asíncrono**        | `async/await`, Promises para I/O y motor de consultas |

---

## 📋 Consultas disponibles en la base de conocimiento

| Predicado                  | Descripción                                       |
|----------------------------|---------------------------------------------------|
| `penalty_applicable(X)`    | Contratos con penalización (retraso + baja calidad) |
| `successful_contract(X)`   | Contratos entregados a tiempo con calidad aprobada |
| `at_risk_contract(X)`      | Contratos sin pago y con retraso                  |
| `reliable_client(X)`       | Clientes con historial positivo o contrato exitoso |
| `high_value_contract(X)`   | Contratos con valor mayor a 50,000                |
| `high_risk_contract(X)`    | Contratos críticos sin pago                       |
| `renewal_recommended(X)`   | Contratos candidatos a renovación                 |
| `audit_required(X)`        | Contratos que requieren auditoría                 |
| `long_term_contract(X)`    | Contratos con duración mayor a 12 meses           |
| `contract_summary(X,...)`  | Resumen completo de un contrato                   |
