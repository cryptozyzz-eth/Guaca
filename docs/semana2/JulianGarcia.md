# Historias de usuario individuales

**Nombre:** Julián García

**Usuario de GitHub:** julian.garc@hotmail.com

---

## Mis historias de usuario

> Entre 5 y 7 historias en formato "como [rol] quiero [acción] para [beneficio]", pensadas desde distintos roles o necesidades del producto que el equipo está diseñando. Si escribes menos de 7, borra las líneas que no uses (mínimo 5).

1. Como data scientist quiero que cada reporte se guarde con campos estructurados (zona, categoría, hora, ubicación, huella de la foto y estado de validación), para tener un conjunto de datos limpio sobre el cual medir y predecir el estado de playas y vías.
2. Como administrador del sistema quiero que un reporte se marque como validado solo cuando lo confirmen varios locales distintos o coincida con datos de clima, para reducir el fraude antes de liberar un pago.
3. Como data scientist quiero calcular un puntaje de confiabilidad por informante a partir de su historial de reportes validados y rechazados, para dar más peso a las fuentes que aciertan.
4. Como administrador del sistema quiero recibir alertas cuando un informante envíe reportes con ubicaciones sospechosas (por ejemplo, saltos de GPS imposibles o varios reportes desde el mismo punto con cuentas distintas), para detectar la falsificación de ubicación antes de que cueste dinero.
5. Como turista quiero ver cada dato con su antigüedad y el número de confirmaciones ("verificado hace 12 min por 3 locales"), para juzgar por mí mismo cuánto confiar en él.
6. Como analista del proyecto quiero medir el tiempo entre el pedido y el pago, el porcentaje de reportes validados sin disputa y el costo por reporte, para comprobar si la hipótesis del Problem Brief se cumple.
7. Como auditor externo (por ejemplo, un hotel u operador) quiero verificar de forma independiente que un reporte no fue alterado ni borrado, para poder recomendar con evidencia y no con publicidad.

## La más importante y por qué

> Organiza las historias de mayor a menor importancia: en la primera fila va la más importante. En cada fila indica el número de la historia y por qué la ubicaste en esa posición. Si usaste menos de 7 historias, borra las filas que sobren.

| Orden de importancia | Historia # | Por qué |
| :---: | :---: | --- |
| 1 (la más importante) | 2 | La cadena garantiza que el registro no cambia, no que el dato sea cierto. Sin una regla de validación, el pago automático se puede explotar. |
| 2 | 1 | Sin datos estructurados desde el primer reporte, nada de lo demás (puntajes, alertas, métricas) es posible después. |
| 3 | 5 | Es lo que el turista ve y entiende. Traduce la validación en confianza y es el diferenciador frente a las reseñas. |
| 4 | 4 | El riesgo más serio del Problem Brief es que falsificar el GPS sea barato. Hay que detectarlo pronto. |
| 5 | 6 | Sin métricas no sabemos si la hipótesis se cumple, pero se pueden calcular a mano en las primeras pruebas. |
| 6 | 3 | Mejora la calidad con el tiempo, pero necesita volumen de reportes antes de que el puntaje signifique algo. |
| 7 (la menos importante) | 7 | Valioso para hoteles y operadores como usuarios futuros, pero no es necesario para validar el ciclo básico de reporte y pago. |
