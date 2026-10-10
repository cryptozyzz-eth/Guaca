# Backlog priorizado (Kanban)

**Proyecto:** Guaca · Semana 2

**Tablero en GitHub Projects:** [Guaca-Backlog MVP] ([https://github.com/users/Hnd-jg/projects/2/views/1])

Este documento respalda el tablero Kanban. Cada historia es un issue del repositorio con sus criterios de aceptación. El orden y la prioridad salen de la sección 1 del [Product Blueprint](ProductBlueprint.md) (método MoSCoW) y el alcance, de la sección 4.

## Convenciones

| Campo | Valores |
| --- | --- |
| Priority | **P0** = Imprescindible · **P1** = Debería · **P2** = Podría |
| Size / Estimate | **S** = 2 · **M** = 3 · **L** = 5 |
| Status | Backlog → Ready → In progress → In review → Done |

## Resumen

| # | Historia | Priority | Size | Estimate | Status | Responsable |
| :---: | --- | :---: | :---: | :---: | :---: | --- |
| H1 | Pago automático al validarse el reporte | P0 | L | 5 | Ready | Sergio |
| H2 | Validación por confirmación de 2 locales distintos | P0 | M | 3 | Ready | Julián |
| H3 | Billetera en el celular para el informante | P0 | M | 3 | Backlog | Sergio |
| H4 | Reporte con foto en pocos toques y estado visible | P0 | M | 3 | Ready | Robert |
| H5 | El patrocinador deposita el fondo antes de publicar el pedido | P0 | M | 3 | Ready | Sergio |
| H6 | Mapa con el estado actual de la zona | P0 | M | 3 | Backlog | Robert |
| H7 | Sello de antigüedad y número de confirmaciones | P0 | S | 2 | Backlog | Robert / Julián |
| H8 | Mapa en el navegador, sin app, sin cuenta y sin pago | P1 | S | 2 | Backlog | Robert |
| H9 | Alertas de ubicaciones sospechosas | P1 | M | 3 | Backlog | Julián |
| H10 | Devolución del fondo si el pedido vence sin validarse | P2 | S | 2 | Backlog | Sergio |

**Distribución:** 7 historias P0, 2 P1 y 1 P2. Ready (14 puntos): H1, H2, H4 y H5, que destraban el ciclo pedido, reporte, validación y pago. Backlog (15 puntos): el resto.

## Detalle de las historias

### [H1] Pago automático al validarse el reporte

**Priority:** P0 · **Size:** L · **Estimate:** 5 · **Status:** Ready · **Responsable:** Sergio  
**Etiquetas:** `historia de usuario`, `must`, `soroban`

#### Historia
Como informante local quiero que el pago se libere automáticamente cuando mi reporte se valida, según reglas públicas, para no depender de que una persona decida si me paga.

**Propuesta por:** Sergio

#### Alcance en el MVP
Zona La Bodeguita e islas, categoría lanchas y estado del mar. Pago en USDC sobre Stellar testnet.

#### Criterios de aceptación
- [ ] Cuando un reporte queda validado (ver H2), la API llama al contrato y este transfiere la recompensa en USDC a la cuenta del informante.
- [ ] El contrato solo paga si el reporte pertenece a un pedido con fondo depositado (H5); si no, la llamada falla y no se mueve dinero.
- [ ] Un mismo reporte no puede pagarse dos veces (hay una prueba que intenta la doble llamada y falla).
- [ ] El monto y el número de confirmaciones exigidas son legibles en el código público del contrato.
- [ ] El hash de la transacción de pago queda guardado junto al reporte y se puede abrir en un explorador de testnet.

#### Depende de
H2, H3, H5

### [H2] Validación por confirmación de 2 locales distintos

**Priority:** P0 · **Size:** M · **Estimate:** 3 · **Status:** Ready · **Responsable:** Julián  
**Etiquetas:** `historia de usuario`, `must`, `backend`

#### Historia
Como administrador del sistema quiero que un reporte se marque como validado solo cuando lo confirmen varios locales distintos o coincida con datos de clima, para reducir el fraude antes de liberar un pago.

**Propuesta por:** Julián, Juan David

#### Alcance en el MVP
Solo confirmación de 2 locales distintos. El cruce con datos de clima queda fuera del MVP.

#### Criterios de aceptación
- [ ] Un reporte pasa a "validado" solo con confirmaciones de al menos 2 cuentas distintas a la del autor.
- [ ] El autor no puede confirmar su propio reporte y una cuenta no puede confirmar dos veces el mismo reporte.
- [ ] Las confirmaciones solo cuentan dentro de una ventana de tiempo definida por el equipo (propuesta: 60 minutos desde el envío).
- [ ] Si la ventana vence sin 2 confirmaciones, el reporte pasa a "no confirmado" y no se paga.
- [ ] Cada cambio de estado (enviado → en validación → validado / no confirmado) queda guardado con fecha y hora.

#### Depende de
H4

### [H3] Billetera en el celular para el informante

**Priority:** P0 · **Size:** M · **Estimate:** 3 · **Status:** Backlog · **Responsable:** Sergio  
**Etiquetas:** `historia de usuario`, `must`, `soroban`, `backend`

#### Historia
Como informante local sin cuenta bancaria quiero recibir el pago en una billetera del celular que pueda crear en pocos minutos, para cobrar montos pequeños sin que las comisiones superen la recompensa.

**Propuesta por:** Juan David, Sergio

#### Alcance en el MVP
La cuenta se crea al primer reporte. Guaca custodia la clave. Sin conversión a pesos (eso es después del MVP).

#### Criterios de aceptación
- [ ] Al enviar su primer reporte, el informante obtiene una cuenta Stellar (testnet) sin salir de la app, en menos de 3 minutos.
- [ ] La reserva mínima de la cuenta la cubre Guaca, de modo que el informante no necesita tener XLM.
- [ ] La cuenta queda con trustline a USDC, lista para recibir pagos.
- [ ] El informante ve su saldo en USDC y su equivalente aproximado en pesos.
- [ ] La clave privada se guarda cifrada en el servidor y nunca aparece en la interfaz ni en los registros (logs).
- [ ] El informante ve un aviso corto que explica que Guaca custodia su clave.

### [H4] Reporte con foto en pocos toques y estado visible

**Priority:** P0 · **Size:** M · **Estimate:** 3 · **Status:** Ready · **Responsable:** Robert  
**Etiquetas:** `historia de usuario`, `must`, `frontend`, `backend`

#### Historia
Como informante local quiero reportar con una foto, la categoría y un botón de enviar, en pocos toques y con el estado visible, para aportar rápido desde el terreno y saber qué pasó con mi reporte.

**Propuesta por:** Juan David, Robert

#### Alcance en el MVP
Una categoría: lanchas y estado del mar.

#### Criterios de aceptación
- [ ] Desde un pedido abierto, el informante envía un reporte en máximo 3 toques: foto, categoría y enviar.
- [ ] La foto se toma con la cámara en ese momento y se guardan la ubicación y la hora del dispositivo.
- [ ] El reporte se guarda con campos estructurados: zona, categoría, hora, ubicación, huella de la foto, autor y estado.
- [ ] La huella (hash SHA-256) de la foto, junto con lugar, hora y autor, se registra en el contrato; la foto se guarda fuera de la cadena.
- [ ] El informante ve el estado de su reporte: enviado, en validación, pagado o rechazado.

### [H5] El patrocinador deposita el fondo antes de publicar el pedido

**Priority:** P0 · **Size:** M · **Estimate:** 3 · **Status:** Ready · **Responsable:** Sergio  
**Etiquetas:** `historia de usuario`, `must`, `soroban`

#### Historia
Como patrocinador de un pedido (hotel u operador) quiero depositar los fondos antes de publicarlo, para que el informante tenga la garantía de que el pago existe.

**Propuesta por:** Sergio

#### Criterios de aceptación
- [ ] El patrocinador crea un pedido con pregunta, zona, recompensa y plazo.
- [ ] El pedido solo aparece publicado cuando el depósito en el contrato está confirmado en la red.
- [ ] El fondo queda bloqueado en el contrato: solo puede salir como pago a un reporte validado (H1) o como devolución al patrocinador según H10.
- [ ] El pedido muestra el monto depositado y el enlace a la transacción del depósito.

### [H6] Mapa con el estado actual de la zona

**Priority:** P0 · **Size:** M · **Estimate:** 3 · **Status:** Backlog · **Responsable:** Robert  
**Etiquetas:** `historia de usuario`, `must`, `frontend`

#### Historia
Como turista quiero abrir un mapa y ver el estado actual de playas, lanchas, vías y eventos en un solo lugar, para decidir mi día sin saltar entre varias fuentes.

**Propuesta por:** Robert

#### Alcance en el MVP
Solo muelle La Bodeguita e islas, categoría lanchas y estado del mar. Playas, vías y eventos quedan para después.

#### Criterios de aceptación
- [ ] El mapa abre centrado en La Bodeguita e islas y muestra los reportes de lanchas y estado del mar.
- [ ] Cada reporte aparece como marcador con su estado (validado, en validación, no confirmado), diferenciado por ícono y color, no solo por color.
- [ ] Solo se muestran reportes recientes, dentro de una ventana definida por el equipo (propuesta: últimas 6 horas).
- [ ] Un reporte nuevo aparece en el mapa sin recargar la página (o con actualización automática cada 60 segundos).
- [ ] El mapa carga en menos de 3 segundos en un celular con 4G.

### [H7] Sello de antigüedad y número de confirmaciones

**Priority:** P0 · **Size:** S · **Estimate:** 2 · **Status:** Backlog · **Responsable:** Robert / Julián  
**Etiquetas:** `historia de usuario`, `must`, `frontend`

#### Historia
Como turista quiero ver en cada dato su antigüedad y el número de confirmaciones ("hace 12 min · 3 locales"), para juzgar por mí mismo cuánto confiar.

**Propuesta por:** Robert, Julián

#### Criterios de aceptación
- [ ] Cada marcador del mapa muestra el sello "hace X min · N locales".
- [ ] La antigüedad se calcula desde la hora en que se tomó la foto, no desde la hora de validación.
- [ ] Los reportes sin validar muestran "sin confirmar" con un estilo distinto al de los validados.
- [ ] Al tocar el sello se ven la foto, la hora y el número de confirmaciones.

#### Depende de
H2, H6

### [H8] Mapa en el navegador, sin app, sin cuenta y sin pago

**Priority:** P1 · **Size:** S · **Estimate:** 2 · **Status:** Backlog · **Responsable:** Robert  
**Etiquetas:** `historia de usuario`, `should`, `frontend`

#### Historia
Como turista quiero usar el mapa desde el navegador, sin instalar una app, sin cuenta y sin pagar, para consultarlo en el momento que lo necesito.

**Propuesta por:** Robert

#### Criterios de aceptación
- [ ] El mapa abre desde una URL pública en el navegador del celular, sin iniciar sesión y sin instalar nada.
- [ ] Funciona en versiones recientes de Chrome (Android) y Safari (iOS).
- [ ] Se puede agregar a la pantalla de inicio (manifiesto PWA).
- [ ] No pide datos personales ni pago en ningún momento.

### [H9] Alertas de ubicaciones sospechosas

**Priority:** P1 · **Size:** M · **Estimate:** 3 · **Status:** Backlog · **Responsable:** Julián  
**Etiquetas:** `historia de usuario`, `should`, `backend`, `datos`

#### Historia
Como administrador del sistema quiero recibir alertas cuando un informante envíe ubicaciones sospechosas, para detectar la falsificación del GPS antes de que cueste dinero.

**Propuesta por:** Julián

#### Alcance en el MVP
Reglas simples y revisión manual. La detección automática avanzada queda para después.

#### Criterios de aceptación
- [ ] La API marca un reporte como "sospechoso" si su ubicación está fuera de la zona del pedido.
- [ ] La API marca como "sospechosos" los reportes enviados desde el mismo punto por cuentas distintas en un intervalo corto (propuesta: 10 minutos).
- [ ] Un reporte sospechoso no libera pago hasta que el administrador lo revise y lo apruebe o lo rechace.
- [ ] El administrador ve una lista de reportes marcados con el motivo de cada alerta.

### [H10] Devolución del fondo si el pedido vence sin validarse

**Priority:** P2 · **Size:** S · **Estimate:** 2 · **Status:** Backlog · **Responsable:** Sergio  
**Etiquetas:** `historia de usuario`, `could`, `soroban`

#### Historia
Como patrocinador de un pedido quiero que los fondos vuelvan a mi billetera si el reporte no se valida a tiempo, para no perder dinero por un pedido que nadie cumplió.

**Propuesta por:** Sergio

#### Alcance en el MVP
Plazo fijo simple: el patrocinador recupera el fondo con una llamada al contrato. La devolución automática queda para después.

#### Criterios de aceptación
- [ ] Si el pedido vence sin un reporte validado, el patrocinador puede recuperar el fondo llamando al contrato.
- [ ] Solo la cuenta que depositó puede recuperarlo, y solo después del plazo.
- [ ] Un fondo que ya se pagó a un informante no se puede recuperar.
