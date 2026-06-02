# Heroes Backend

API REST construida con NestJS que sirve los datos de superhéroes y villanos para la aplicación [heroes-app](../heroes-app).

## Stack tecnológico

| Tecnología | Versión | Uso |
|---|---|---|
| NestJS | 11 | Framework |
| TypeScript | 5.7 | Tipado estático |
| class-validator | 0.14 | Validación de DTOs |
| class-transformer | 0.5 | Transformación de tipos |
| Jest + Supertest | 29 / 7 | Tests |

## Funcionalidades

- **CRUD completo** de héroes con validación de entrada
- **Listado paginado** con filtro por categoría
- **Búsqueda avanzada** por nombre, equipo, categoría, universo, estado y fuerza mínima
- **Dashboard / resumen** con métricas: total, héroe más fuerte, más inteligente, conteo por categoría
- Almacenamiento **en memoria** (datos precargados desde `src/data/heroes.data.ts`)
- **CORS** habilitado para conexión con el frontend
- **ValidationPipe** global con transformación implícita de tipos

## Instalación y desarrollo

```bash
# 1. Instalar dependencias
npm install

# 2. Arrancar en modo watch (recompila con cada cambio)
npm run start:dev
```

La API estará disponible en `http://localhost:3000/api`.

## Variables de entorno

| Variable | Por defecto | Descripción |
|---|---|---|
| `PORT` | `3000` | Puerto del servidor |

```bash
# Ejemplo: arrancar en el puerto 3001
$env:PORT=3001; npm run start:dev   # PowerShell
PORT=3001 npm run start:dev         # bash/macOS/Linux
```

## Scripts disponibles

```bash
npm run start         # Producción (compilado previo requerido)
npm run start:dev     # Desarrollo con watch
npm run start:debug   # Debug con inspector de Node.js
npm run start:prod    # Producción desde dist/
npm run build         # Compila el proyecto a dist/
npm run test          # Tests unitarios
npm run test:watch    # Tests en modo watch
npm run test:cov      # Tests con cobertura
npm run test:e2e      # Tests end-to-end
npm run lint          # Linter con ESLint + Prettier
npm run format        # Formateado con Prettier
```

## Endpoints de la API

Base URL: `/api`

### Héroes

| Método | Ruta | Descripción |
|---|---|---|
| `GET` | `/api/heroes` | Listado paginado |
| `GET` | `/api/heroes/summary` | Métricas del dashboard |
| `GET` | `/api/heroes/search` | Búsqueda avanzada |
| `GET` | `/api/heroes/:id` | Héroe por ID o slug |
| `POST` | `/api/heroes` | Crear héroe |
| `PATCH` | `/api/heroes/:id` | Actualizar héroe |
| `DELETE` | `/api/heroes/:id` | Eliminar héroe |

### Query params — `GET /api/heroes`

| Param | Tipo | Descripción |
|---|---|---|
| `limit` | number (≥ 1) | Héroes por página. Por defecto: `6` |
| `offset` | number (≥ 0) | Desplazamiento. Por defecto: `0` |
| `category` | string | Filtra por categoría (`Hero`, `Villain`, `all`). Por defecto: `all` |

**Respuesta:**
```json
{
  "total": 42,
  "pages": 7,
  "heroes": [...]
}
```

### Query params — `GET /api/heroes/search`

Al menos un parámetro es obligatorio.

| Param | Tipo | Descripción |
|---|---|---|
| `name` | string | Filtra por nombre o alias |
| `team` | string | Filtra por equipo |
| `category` | string | Filtra por categoría |
| `universe` | string | Filtra por universo |
| `status` | string | Filtra por estado |
| `strength` | number | Fuerza mínima |

### Modelo Hero

```typescript
{
  id: string
  name: string
  slug: string
  alias: string
  powers: string[]
  description: string
  strength: number       // 0–10
  intelligence: number   // 0–10
  speed: number          // 0–10
  durability: number     // 0–10
  team: string
  image: string
  firstAppearance: string
  status: string         // 'Active' | 'Inactive' | 'Deceased'
  category: string       // 'Hero' | 'Villain'
  universe: string       // 'Marvel' | 'DC' | ...
}
```

## Estructura del proyecto

```
src/
├── common/
│   └── dto/
│       └── pagination.dto.ts     # DTO paginación reutilizable
├── data/
│   └── heroes.data.ts            # Dataset inicial en memoria
├── heroes/
│   ├── dto/
│   │   ├── create-hero.dto.ts
│   │   ├── update-hero.dto.ts
│   │   └── advande-search.dto.ts
│   ├── entities/
│   │   └── hero.entity.ts
│   ├── heroes.controller.ts
│   ├── heroes.service.ts
│   └── heroes.module.ts
├── app.module.ts
└── main.ts
```
