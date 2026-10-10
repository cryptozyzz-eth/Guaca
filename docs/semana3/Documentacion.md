# Documentación — Semana 3

## 1. Front construido

El desarrollo del producto se hace en el repositorio [Cartagenaonchain/guacalive](https://github.com/Cartagenaonchain/guacalive). Este repositorio (Guaca) reúne los documentos formales del curso y enlaza a ese código. Preferimos mantenerlo así, por orden y con respeto: guacalive parte del trabajo de [guacamap/guaca-app](https://github.com/guacamap/guaca-app), y no queríamos dejar su código fuente copiado aquí como un fork. Agradecemos a su autora. Los pasos para ejecutar el front están en [frontend/README.md](../../frontend/README.md).

Lo que sigue describe las pantallas de guacalive según su README y su documento de producto (`PRODUCT.md`), y cómo se relacionan con el MVP del [Product Blueprint](../semana2/ProductBlueprint.md). No describe pantallas que no hemos visto funcionando.

**Pantallas disponibles en guacalive**

- **Página de presentación** (`localhost:3000`): sitio de marca y páginas legales, hecho en Next.js.
- **Mapa** (`localhost:3002/map`): mapa con lugares verificados de la zona cubierta, en una aplicación web instalable (PWA), pensada para celular y conexiones lentas.
- **Aplicación del informante, "Spotter"** (`localhost:3002/spotter`): pantalla en español donde un local recibe misiones pagadas, ubica y fotografía un lugar y lo verifica.
- **Vista por código QR** (`localhost:3002/v/qr-marina`): vista que un hotel o posada muestra a sus huéspedes para que consulten sin instalar nada ni pagar.
- **Administración** (`localhost:3003`, con token): un operador humano revisa vacíos de cobertura, misiones, informantes y verificaciones.
- **API** (`localhost:3001`): servicio que conecta las pantallas con la base de datos.

**Cómo se conectan con el MVP del Blueprint**

El MVP de la sección 4 del Blueprint recorre cinco pasos. Esta es la correspondencia con lo que existe hoy:

| Paso del MVP | Qué hay en guacalive | Brecha |
| :---- | :---- | :---- |
| El patrocinador publica un pedido y deposita el fondo | El sistema detecta vacíos de cobertura y crea misiones pagadas. El dueño del inmueble figura con un plan, que solo sirve como señal de demanda. | No hay depósito ni cobro: no existe un flujo de pago del patrocinador. |
| El informante reporta con foto y ve su estado | La aplicación del informante cubre ubicar, fotografiar y verificar un lugar. | Falta confirmar si muestra el estado del reporte y el saldo en pesos. |
| Validación por la confirmación de 2 locales | Hay verificación por agente y por operador. | La regla de dos locales distintos no es la regla actual. |
| Pago automático al validarse | Los pagos pasan por una sola interfaz, `PayoutProvider`; hoy solo funciona la versión simulada. | Es la pieza que Stellar vendría a reemplazar. |
| Mapa con sello de antigüedad y confirmaciones | El mapa y la vista QR muestran lugares verificados con niveles de confianza. | Falta confirmar que muestre "hace X min · N locales". |

Dos diferencias importantes entre guacalive y nuestro MVP:

1. **Zona y datos.** guacalive modela hoy Puerto Cabello (Venezuela) con datos ficticios. Nuestro MVP es una zona de Cartagena (muelle La Bodeguita e islas) y la categoría lanchas y estado del mar.
2. **Cadena.** El producto actual declara "sin token y sin cadena". Nuestra propuesta añade Stellar como proveedor de pagos y registro detrás de `PayoutProvider`, sin cambiar las reglas de producto de guacalive.

Capturas de pantalla: pendientes de tomar al ejecutar guacalive localmente. Enlace hospedado: no hay todavía.

## 2. Decisión técnica

**Opción elegida: A, contrato propio en Soroban (la plataforma de contratos de Stellar), solo en testnet.** En esta semana solo se documenta; no se escribe ni se despliega código.

**Qué registraría y ejecutaría el contrato**

- **Fondo del pedido:** guarda el dinero que el patrocinador deposita para un pedido, bloqueado hasta que se cumpla o venza.
- **Huella del reporte:** registra el hash de la foto junto con el lugar, la hora y el autor. La foto en sí queda fuera de la cadena, en el almacén de archivos.
- **Confirmaciones:** cuenta las confirmaciones de locales distintos.
- **Pago:** al llegar a dos confirmaciones, paga automáticamente al informante.

**Por qué hace falta un contrato propio.** El criterio de pertinencia del Problem Brief dice que quien opera una base de datos es parte interesada: podría borrar un reporte negativo o cambiar un pago ya cumplido sin que nadie lo detecte. Un contrato con reglas públicas deja el fondo, el registro y la regla de pago a la vista de todos. Ninguna herramienta existente combina estas tres cosas con la regla de "dos locales distintos".

**Dónde encaja en guacalive.** Su único punto de pago es la interfaz `PayoutProvider`. La idea es añadir una versión que hable con Stellar, sin tocar el resto: la misión pagada sería el pedido con fondo, y el pago al informante se haría en USDC con una cuenta creada al primer reporte.

**Alternativas descartadas**

- **Opción B, solución existente de Stellar** (por ejemplo, saldos reclamables y pagos nativos): sirve para pagos simples, pero no cuenta confirmaciones ni registra la huella.
- **Base de datos tradicional con auditoría:** más barata, pero la controla quien la opera.
- **Pasarelas de pago tradicionales:** las comisiones superan la recompensa y exigen cuenta bancaria.

**Riesgo conocido.** La billetera del informante la custodia Guaca para que cobrar sea fácil. Eso reintroduce confianza en la custodia, aunque el fondo y la regla de pago siguen siendo públicos.

## 3. Participación del equipo

| Integrante | Usuario de GitHub | Aporte |
| :---- | :---- | :---- |
| Julián García | julian.garc@hotmail.com | Pendiente de completar |
| Robert López | nwrobj@gmail.com | Pendiente de completar |
| Sergio Martínez Marín | sergiotechx@yahoo.com | Pendiente de completar |
| Juan David Correa | cartagenaonchain@gmail.com | Pendiente de completar |

Los usuarios son los correos registrados en la semana 1; faltan los nombres de usuario reales de GitHub.

## 4. Bloqueos y siguiente paso

**Bloqueos**

- El front corre desde guacalive, no desde este repositorio; quien califique debe seguir el enlace. Para reducirlo, el README da los pasos exactos y falta un enlace hospedado.
- Ejecutar guacalive exige pnpm y Docker, que aún no están en todos los equipos del grupo.
- guacalive modela Puerto Cabello con datos ficticios; falta adaptarlo a Cartagena.
- Falta confirmar la hora límite exacta de la entrega.

**Siguiente paso:** correr guacalive y tomar las capturas, definir con la autora y el equipo cómo se integra la versión de Stellar, y escribir el contrato en testnet.
