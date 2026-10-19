# Guaca

*Cartagena, contada por quien la vive.*

Guaca es un mapa en vivo del Caribe donde los locales reciben pago por reportes verificados y en tiempo real sobre playas, lanchas, vías y eventos, y cada reporte y cada pago queda registrado en cadena para que nadie pueda alterarlo después.

[Read this in English](README.en.md)

## El problema

En Cartagena las condiciones cambian de un día a otro: el mar se pica, la Capitanía de Puerto suspende los zarpes a las islas, una vía se congestiona o aparece un evento en Getsemaní. Quienes lo saben primero son lancheros, vendedores de playa, mototaxistas y guías, y lo cuentan en grupos de WhatsApp. Al día siguiente el mensaje queda enterrado y no llega a ningún mapa.

Mientras tanto:

- **El turista** decide su día con información incompleta o vieja: reseñas de Tripadvisor y Google Maps que pueden tener años, o recomendaciones de intermediarios ("jaladores") que cobran comisión por sugerir un operador concreto.
- **El informante local**, que sabe lo que realmente está pasando, regala ese conocimiento gratis, sin reconocimiento ni pago, muchas veces sin ni siquiera tener cuenta bancaria para recibir una pequeña recompensa.
- **Las plataformas de reseñas** concentran la confianza como árbitro único, cobran a hoteles y operadores por aparecer, y nadie fuera de ellas puede verificar si un reporte fue alterado o borrado.

El resultado: el dato fresco se evapora en horas, la información vieja o falsa compite en igualdad de condiciones con la verdadera, y quien produce el valor real —el local que observa el terreno— no capta nada de ese valor.

## Por qué esto necesita blockchain

Una base de datos tradicional no alcanza porque quien la operaría es parte interesada: si cobra a negocios por aparecer y a la vez decide qué reportes se publican y a quién se paga, podría alterar un reporte negativo o cambiar el valor de un pedido ya cumplido, y nadie lo detectaría. El caso cumple tres condiciones que justifican un registro descentralizado:

1. **Partes que no confían entre sí necesitan compartir un mismo registro** — negocios, informantes y turistas quieren leer el mismo historial de reportes y pagos, sin que ninguno pueda reescribirlo unilateralmente.
2. **El histórico no puede alterarse después** — un sello de "verificado" solo vale si su marca de tiempo y evidencia son inmutables.
3. **Se elimina un intermediario que hoy concentra la confianza** — un contrato con reglas de pago públicas reemplaza la promesa de una empresa por un compromiso verificable por cualquiera.

## La solución

Cada solicitud de información ("¿están saliendo lanchas desde La Bodeguita?") se convierte en un reporte con autor identificable, evidencia (foto con ubicación y hora) y una huella digital registrada en cadena. Cuando el reporte se valida —por ejemplo, con confirmación de varios locales o cruce con datos de clima— un contrato inteligente libera automáticamente el pago al informante, en minutos y sin necesidad de una cuenta bancaria tradicional.

Con esto:

- **El turista** ve un sello como "verificado hace 12 minutos por 3 locales" y puede comprobarlo por sí mismo, en vez de confiar ciegamente en una reseña.
- **El informante local** cobra en minutos, con reglas públicas que nadie puede cambiar a su favor, y construye una reputación propia y portable.
- **El hotel u operador** puede recomendar con evidencia verificable en vez de depender de publicidad pagada.

## Beneficios

| Para | Beneficio |
| :---- | :---- |
| Turista | Información fresca y verificable en vez de reseñas viejas o infladas; evita perder un día de viaje por un dato equivocado. |
| Informante local | Recibe pago casi inmediato por un conocimiento que hoy regala gratis; construye reputación propia, no de una plataforma. |
| Hoteles y operadores | Pueden recomendar con evidencia, no con comisión; menos exposición a reseñas falsas. |
| Ecosistema turístico | Reduce la dependencia de intermediarios que cobran por concentrar confianza; el registro es auditable por cualquiera. |

## Cómo funciona: antes y después

### Flujo actual (sin Guaca)

```mermaid
flowchart TD
    A[Ocurre el hecho en el terreno<br/>ej: mar picado, zarpe suspendido] --> B[Local lo observa<br/>y lo comenta en WhatsApp]
    B --> C[Mensaje se pierde:<br/>sin fecha ni ubicación buscable]
    D[Turista busca en<br/>Google Maps / Tripadvisor] --> E[Encuentra reseñas<br/>viejas o posiblemente falsas]
    E --> F[Turista pregunta a hotel,<br/>taxista o jalador]
    F --> G[Recomendación con<br/>posible conflicto de interés<br/>por comisión]
    G --> H[Turista decide y paga<br/>al operador]
    H --> I[Local con el dato correcto<br/>no recibe nada]

    style C fill:#f8d7da,stroke:#dc3545
    style I fill:#f8d7da,stroke:#dc3545
    style G fill:#fff3cd,stroke:#ffc107
```

### Flujo propuesto (con Guaca)

```mermaid
flowchart TD
    A[Ocurre el hecho en el terreno<br/>ej: mar picado, zarpe suspendido] --> B[Local crea reporte:<br/>foto + ubicación + hora]
    B --> C[Huella digital del reporte<br/>se registra en cadena]
    C --> D{Reporte se valida:<br/>varios locales confirman<br/>o cruce con datos de clima}
    D -->|Validado| E[Contrato inteligente<br/>libera pago automático<br/>al informante]
    D -->|No validado| F[Reporte queda marcado<br/>como no confirmado]
    E --> G[Local recibe pago<br/>en minutos, sin banco]
    C --> H[Turista ve el reporte<br/>con sello: 'verificado hace<br/>12 min por 3 locales']
    H --> I[Turista decide con<br/>evidencia verificable]

    style C fill:#d4edda,stroke:#28a745
    style E fill:#d4edda,stroke:#28a745
    style G fill:#d4edda,stroke:#28a745
    style I fill:#d4edda,stroke:#28a745
```

## Cómo ejecutarlo

El código del producto vive en un repositorio aparte, [cryptozyzz-eth/Guaca-Core](https://github.com/cryptozyzz-eth/Guaca-Core), que parte del trabajo de [guacamap/guaca-app](https://github.com/guacamap/guaca-app); agradecemos a su autora. Este repositorio reúne los reportes y documentos formales del curso y enlaza al código, para no mezclarlos con él. Todo lo que usa la red de Stellar corre en testnet.

**Requisitos:** Node 24 o superior, pnpm 11 o superior y Docker.

**Instalación y ejecución** (desde un clon de Guaca-Core):

```bash
pnpm install
docker compose up -d
pnpm demo:prepare
pnpm dev
```

El mapa queda en `http://localhost:3002/map` y la aplicación del informante en `http://localhost:3002/spotter`. Los detalles están en [frontend/README.md](frontend/README.md).

## Estructura del repositorio

```text
.
├── docs/
│   ├── semana1/        # Problem Brief y propuestas individuales
│   ├── semana2/        # Product Blueprint, historias, Lean Canvas
│   ├── semana3/        # Documentacion.md
│   └── semana4/        # MVPFunctional.md
├── frontend/           # Enlace y pasos para ejecutar el front (código en Guaca-Core)
└── README.md
```

## Estado del proyecto

Guaca está en etapa temprana de validación de problema (Entregable 1: Problem Brief). El equipo son residentes de Cartagena que están validando la hipótesis directamente en la ciudad.

## Equipo

| Integrante | Rol |
| :---- | :---- |
| Julián García | Data scientist y BD |
| Robert López | Frontend y experiencia de usuario |
| Sergio Martínez Marín | Contratos inteligentes y backend |
| Juan David Correa | BD, investigación de usuario y comunidad local |
