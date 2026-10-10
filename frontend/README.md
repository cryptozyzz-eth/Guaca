# Front de Guaca

El código del producto, incluido el front, se desarrolla en un repositorio aparte: **[Cartagenaonchain/guacalive](https://github.com/Cartagenaonchain/guacalive)**.

Este repositorio (Guaca) reúne los documentos formales del curso (`docs/semana1` a `docs/semana5`) y enlaza al código. Se decidió mantenerlo así para no duplicar ni dejar como copia el código fuente, que parte del trabajo original de [guacamap/guaca-app](https://github.com/guacamap/guaca-app).

## Cómo ejecutar el front

Requisitos: Node 24 o superior, pnpm 11 o superior y Docker.

```bash
git clone https://github.com/Cartagenaonchain/guacalive.git
cd guacalive
pnpm install
docker compose up -d      # PostgreSQL con PostGIS y MinIO
pnpm demo:prepare         # compila, migra y carga los datos de demostración
pnpm dev                  # levanta todas las aplicaciones
```

Direcciones locales:

| Pantalla | Dirección |
| :---- | :---- |
| Página de presentación | http://localhost:3000 |
| Mapa | http://localhost:3002/map |
| Aplicación del informante (Spotter) | http://localhost:3002/spotter |
| Administración (con token) | http://localhost:3003 |
| API | http://localhost:3001 |

Los pasos salen del README de guacalive; si cambian allá, esa es la fuente que vale.

## Enlace hospedado

Pendiente: no hay una versión publicada todavía.
