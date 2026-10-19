# Documentación — Semana 3

> Actualizada en la semana 4. En la primera versión declaramos la opción A (contrato propio en Soroban). Al construir la integración vimos que la opción B cubría lo necesario para probar el ciclo de pago en testnet, y la decisión técnica quedó corregida abajo con su razón. El código se trabaja en [Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core) y este repositorio conserva los reportes del curso (ver la nota en [docs/semana4/MVPFunctional.md](../semana4/MVPFunctional.md)).

## 1. Front construido

El front vive en el repositorio de código [Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core), en `apps/app` (aplicación web instalable, PWA, hecha con Next.js), con un sitio de presentación en `apps/web` y un panel de operación en `apps/admin`. Los pasos para ejecutarlo están en el [README de Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core#running-it) y se resumen en [frontend/README.md](../../frontend/README.md). El proyecto parte de [guacamap/guaca-app](https://github.com/guacamap/guaca-app), bajo licencia Apache 2.0, y agradecemos a su autora.

Lo que sigue describe las pantallas según el código y la documentación del repositorio, y cómo se relacionan con el MVP del [Product Blueprint](../semana2/ProductBlueprint.md).

**Pantallas**

- **Presentación** (`apps/web`, `localhost:3000`): sitio de marca con páginas legales.
- **Portada y elección de rol** (`apps/app`): punto de entrada de la aplicación, con un selector de rol.
- **Mapa** (`/map`): lugares con su nivel de confianza (verificado por locales, corroborado por datos abiertos o solo listado), pensado para celular y conexiones lentas.
- **Vista por código QR** (`/v/[código]` y `/qr-card/[código]`): lo que un hotel o posada muestra a sus huéspedes para consultar sin instalar nada ni pagar.
- **Aplicación del informante, "Spotter"** (`/spotter`, en español): misiones pagadas, reporte con foto y ubicación, historial de cobros y, desde la semana 4, la tarjeta "Activar cobros" que crea la billetera.
- **Comercio** (`/merchant`): vista pensada para el negocio que patrocina (por confirmar al ejecutarla).
- **Administración** (`apps/admin`, `localhost:3003`, con token): un operador revisa vacíos de cobertura, misiones, informantes y verificaciones, y puede ordenar el pago de una misión verificada.
- **API** (`apps/api`, `localhost:3001`): conecta las pantallas con la base de datos y con Stellar.

**Cómo se conectan con el MVP del Blueprint**

| Paso del MVP | Qué hay hoy | Brecha |
| :---- | :---- | :---- |
| El patrocinador publica un pedido y deposita el fondo | El sistema detecta vacíos de cobertura y crea misiones pagadas; el comercio aparece con un plan que sirve como señal de demanda. | No hay depósito ni cobro al patrocinador. |
| El informante reporta con foto y ve su estado | La aplicación del informante cubre ubicar, fotografiar y verificar un lugar, y muestra el historial con el estado de cada misión. | Falta confirmar el saldo en pesos. |
| Validación por la confirmación de 2 locales | Hay verificación por agentes y por operador, y una confirmación por un segundo informante cercano. | La regla de dos locales distintos como condición del pago no está cableada. |
| Pago automático al validarse | Una misión verificada se paga desde el panel del operador por la interfaz `PayoutProvider`; si el informante activó su billetera, el pago sale en USDC de testnet. | El disparo es manual, no automático. |
| Mapa con sello de antigüedad y confirmaciones | El mapa muestra lugares con niveles de confianza. | Falta el sello "hace X min · N locales". |

Dos diferencias entre el código heredado y nuestro MVP:

1. **Zona y datos.** El proyecto modelaba Puerto Cabello (Venezuela). Añadimos misiones de demostración de Cartagena y Medellín para el MVP, con datos ficticios.
2. **Cadena.** El producto original declaraba "sin token ni cadena". Añadimos Stellar como un carril de pagos solo en testnet detrás de `PayoutProvider`: quien no activa su billetera sigue en el pago simulado.

Capturas de pantalla: pendientes. Enlace en vivo: no hay todavía.

## 2. Decisión técnica

**Opción elegida: B, tecnología existente del ecosistema Stellar, solo en testnet.** Usamos Pollar (infraestructura de billeteras de Stellar con abstracción de cuentas) para crear la billetera del informante, y pagos nativos de Stellar en USDC de testnet. No escribimos ni desplegamos contratos propios.

**Para qué sirve cada pieza**

- **Pollar:** el informante crea su billetera con un código por correo, sin frase semilla y con comisiones patrocinadas. Responde al supuesto del Problem Brief de que el local no tiene cuenta bancaria ni sabe de claves.
- **Pagos nativos de Stellar en USDC:** una cuenta tesorería paga la recompensa al informante, con un memo de idempotencia para no pagar dos veces, y guardamos el hash de la transacción para mostrar un comprobante público.

**Por qué no un contrato propio (todavía).** Un contrato Soroban encaja con el Blueprint (fondo bloqueado del patrocinador, huella del reporte y pago por dos confirmaciones), pero para esta etapa no hacía falta: el objetivo era probar que el informante cobra en minutos y sin banco, y un pago nativo lo prueba con menos código, menos riesgo y costo de centavos. La contrapartida es honesta: la tesorería central reintroduce confianza en quien la opera, y esa es la razón por la que el contrato sigue siendo el siguiente paso.

**Alternativas descartadas**

- **Contrato Soroban propio ahora:** más trabajo y más superficie de error para el plazo disponible; queda como trabajo futuro.
- **Base de datos tradicional con auditoría:** la controla quien la opera, que podría cambiar un pago o borrar un reporte.
- **Pasarelas de pago tradicionales:** las comisiones superan la recompensa y exigen cuenta bancaria.

**Riesgo conocido.** El enlace entre el usuario y su billetera no se verifica contra Pollar en esta demostración; en producción debe verificarse.

## 3. Participación del equipo

Las decisiones se acuerdan en reuniones del equipo, donde cada integrante aporta conocimientos complementarios (origen del proyecto, análisis de datos, desarrollo de negocio y desarrollo técnico). Lo publicado en GitHub ha sido hecho principalmente por la cuenta `sergiotechx`, que registra los commits de la integración con Stellar y Pollar.

## 4. Bloqueos y siguiente paso

**Bloqueos**

- No hay producto en línea; ejecutarlo exige Node 24, pnpm 11, Docker y claves de prueba.
- El pago lo ordena un operador; falta automatizarlo con la regla de dos confirmaciones.
- El fondo del patrocinador no existe todavía.
- Faltan las capturas de pantalla.

**Siguiente paso:** desplegar el producto, escribir el contrato Soroban con el fondo del patrocinador y la regla de dos confirmaciones, y validar con informantes y patrocinadores (ver el plan de validación en la semana 4).
