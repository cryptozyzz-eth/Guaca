# MVP funcional — Semana 4

**Proyecto:** Guaca · **Repositorio de entrega del curso (documentación):** [cryptozyzz-eth/Guaca](https://github.com/cryptozyzz-eth/Guaca) · **Código del producto:** [cryptozyzz-eth/Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core)

Todo lo que se ejecuta en esta entrega corre en la **red de pruebas (testnet) de Stellar**. Los fondos son USDC de prueba, sin valor real, y los datos de las misiones son de demostración.

## Nota sobre los repositorios

Guaca es un desarrollo activo, y por eso separamos el trabajo en dos repositorios con funciones distintas:

- **Guaca** (este repositorio) reúne los reportes y entregables del curso: documentación de las semanas 1 a 5 y el README. Lo mantenemos aparte para que los informes del curso no se mezclen con el código del proyecto.
- **[Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core)** contiene el código del producto (aplicación, API, base de datos y pagos). Sigue su propio ritmo de desarrollo.

La razón técnica de llevar el código a su propio repositorio es que integrar Stellar de forma nativa cambia el esquema de datos (dirección de billetera, hash de la transacción), la interfaz de pagos `PayoutProvider` y la configuración del servidor. Versionarlo aparte nos da libertad para diseñar esa integración y no condiciona ni genera conflictos con los trabajos anteriores, de los que reconocemos el origen: el proyecto parte de [guacamap/guaca-app](https://github.com/guacamap/guaca-app) y conserva su licencia Apache 2.0. Agradecemos a su autora.

La integración con Stellar nace del curso **Blockchain Builders 101**, que nos dio el marco para pasar de una idea con pagos simulados a un pago real en testnet.

## Evidencias verificables

| Evidencia | Enlace |
| :---- | :---- |
| Producto en vivo | Pendiente: hoy corre en local (ver Riesgos y pendientes) |
| Video de la demostración (máximo 3 minutos) | Pendiente de publicar |
| Pago de 3 USDC en testnet | [Transacción 772a2d1d…9396](https://stellar.expert/explorer/testnet/tx/772a2d1d861987692ad150dd6314546d992ea28932f8e65a94ef2978d8099396) |
| Billetera del informante creada con Pollar | [Cuenta GANNAM2Q…SXXMB](https://stellar.expert/explorer/testnet/account/GANNAM2QZNOXJM3FLP43UWK3PU6YEKXONUR6JDHOHN7LA7DG45MSXXMB) |
| Creación de esa cuenta, con comisiones patrocinadas | [Transacción cca05c7b…986e](https://stellar.expert/explorer/testnet/tx/cca05c7b1e99893b14d398473b68fb533439769d32a33a300c373437acb6986e) |
| Cuenta tesorería que paga | [Cuenta GARFKPAI…KKHA](https://stellar.expert/explorer/testnet/account/GARFKPAIMYCB6LJHH4DPTWOWYINGKLTHYNHCSVJI6KTPVROD7KMDKKHA) |

Declaración técnica: usamos la **opción B** (tecnología existente del ecosistema Stellar): Pollar (infraestructura de billeteras de Stellar) para crear las billeteras con abstracción de cuentas y pagos nativos de Stellar en USDC de testnet. No hay contratos propios desplegados. El código está en Guaca-Core: [`apps/api/src/pollar.ts`](https://github.com/cryptozyzz-eth/Guaca-Core/blob/main/apps/api/src/pollar.ts), [`apps/app/src/components/PayoutsCard.tsx`](https://github.com/cryptozyzz-eth/Guaca-Core/blob/main/apps/app/src/components/PayoutsCard.tsx) y [`packages/db/src/payout/`](https://github.com/cryptozyzz-eth/Guaca-Core/tree/main/packages/db/src/payout).

## 1. Aprendizajes y ajustes priorizados

**Qué aprendimos**

1. **Pollar quitó la fricción de la billetera.** El informante activa sus cobros con un código por correo, sin frase semilla ni comisiones que pagar. Era el primer supuesto del Problem Brief y ahora lo comprobamos técnicamente en testnet.
2. **Web2 y web3 rinden más cuando cada una hace lo suyo.** El mapa, las fotos, los agentes y la verificación son rápidos y baratos en web2. El pago y su comprobante público son donde la cadena aporta. Llevarlo todo a la cadena habría encarecido cada reporte.
3. **Un pago nativo de Stellar basta para probar el ciclo.** Cuesta centavos y deja un comprobante que cualquiera abre. El contrato propio aportaría más, pero no era necesario para validar la idea.
4. **La tesorería central es el punto débil**, porque reintroduce la confianza que queríamos quitar.

**Ajustes, por impacto y esfuerzo**

- *Antes del cierre (impacto alto, esfuerzo bajo):* declarar la opción B, enlazar el pago y las cuentas en el explorador, y mostrar al informante su comprobante.
- *Pendientes:* contrato Soroban con el fondo del patrocinador (impacto y esfuerzo altos); confirmación de dos locales (medio, medio); producto en línea (alto, medio); alertas de ubicaciones sospechosas (medio, medio); conversión a pesos con anclas (medio, alto).

## 2. Riesgos y pendientes

- **Tesorería central.** Una sola cuenta paga a todos con una llave secreta guardada en una variable de entorno. Si se filtra o un operador paga por error, se pierde la garantía. Mitigación actual: solo testnet y la llave fuera del repositorio. Solución de fondo: un contrato con fondo bloqueado.
- **Billetera sin verificar.** La API vincula la dirección que reporta la aplicación; en producción debe verificarse con Pollar antes de aceptarla.
- **Solo testnet y datos ficticios.** El USDC es de prueba. Las misiones de Cartagena y Medellín son de demostración y no hay verificaciones reales de informantes.
- **Brecha con el MVP del Blueprint.** El pago lo dispara un operador tras la verificación. Faltan la confirmación de dos locales distintos y el depósito del fondo por el patrocinador.
- **Ubicación falsificable.** Un reporte con GPS falso podría cobrar una recompensa. Hoy depende de la revisión del operador.
- **Sin producto en línea.** Corre en local y requiere Node 24, pnpm 11, Docker y claves de prueba. Desplegarlo es el siguiente paso.
- **Dependencias externas.** Pollar, el emisor del USDC de prueba y Horizon. Un cambio o una caída afecta los cobros.
- **Regulación.** No hemos resuelto la normativa colombiana sobre activos digitales ni la conversión a pesos.
- **Commit individual.** El trabajo se acuerda en reuniones del equipo y lo publicado en GitHub lo hace principalmente una cuenta; no todos los integrantes tienen commit propio esta semana. Lo consultaremos con el docente.

## 3. Plan de validación con usuarios

**A quién.** Al informante local (lanchero, vendedor de playa, mototaxista, guía) y al turista del Problem Brief, que decide su día con información incompleta. Como pagadores de los pedidos, a hoteles y operadores.

**Cómo.**

1. *Entrevistas a informantes* en el muelle La Bodeguita y en Playa Blanca: 8 a 10 personas, con una prueba guiada de activar cobros, reportar y ver su comprobante.
2. *Piloto con patrocinadores:* 3 a 5 hoteles u operadores publican un pedido pequeño de prueba y muestran a sus huéspedes la vista por código QR.
3. *Métricas* del Problem Brief, tomadas en cada prueba.

**Hipótesis y criterios de confirmación**

| Hipótesis | Criterio para confirmarla |
| :---- | :---- |
| El informante activa sus cobros sin ayuda. | Al menos 7 de 10 lo logran en menos de 5 minutos. |
| Cobrar en minutos y sin banco le resulta valioso. | El tiempo entre verificación y pago es menor a 10 minutos y al menos 7 de 10 dicen que volverían a reportar por esa recompensa. |
| El costo por transacción no se come la recompensa. | El costo por pago es una fracción pequeña de la recompensa (meta: menos de 5 %). |
| El turista confía más en un dato verificado que en una reseña. | La mayoría prefiere el dato con sello en una comparación directa. |
| Un patrocinador pagaría por un pedido. | Al menos 2 de 5 aceptan financiar un segundo pedido. |

**Cuándo descartamos o ajustamos.** Si menos de la mitad de los informantes activa sus cobros sin ayuda, rediseñamos el proceso. Si ningún patrocinador acepta pagar, cambiamos el modelo de ingresos antes de construir más. Los resultados se registran en un documento nuevo en `docs/` y se revisan con el equipo.
