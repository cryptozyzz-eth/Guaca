# Front de Guaca

El código del producto, incluido el front, se desarrolla en un repositorio aparte: **[cryptozyzz-eth/Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core)**.

Este repositorio (Guaca) reúne los documentos formales del curso (`docs/semana1` a `docs/semana5`) y enlaza al código. Se mantiene así para que los reportes del curso no se mezclen con el código del proyecto, que parte del trabajo original de [guacamap/guaca-app](https://github.com/guacamap/guaca-app).

## Cómo ejecutar el front

Requisitos: Node 24 o superior, pnpm 11 o superior y Docker.

```bash
git clone https://github.com/cryptozyzz-eth/Guaca-Core.git
cd Guaca-Core
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

Para probar los cobros en la red de pruebas de Stellar (testnet) se necesitan claves de prueba de Pollar y una cuenta tesorería de testnet; los nombres de las variables están en `apps/api/.env.example` y `apps/app/.env.example` de Guaca-Core. Nunca se suben valores reales al repositorio.

Los pasos salen del README de Guaca-Core; si cambian allá, esa es la fuente que vale.

## Enlace hospedado

Pendiente: no hay una versión publicada todavía.
