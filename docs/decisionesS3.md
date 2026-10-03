# Decisiones de diseño — Semana 3

## 1. Punto de entrada

El proyecto mantiene un único punto de entrada:

```text
IngestaSensores.main()
```

No se crean aplicaciones independientes por semana.

`BancoDePruebas` es una clase auxiliar y no contiene `main`.

## 2. Búsqueda por timestamp

Se utilizan dos estrategias:

- Búsqueda lineal: no requiere ordenamiento.
- Búsqueda binaria: requiere que el arreglo esté ordenado por timestamp.

Los datos sintéticos de `GeneradorDatos` se generan en orden cronológico, por lo que la búsqueda binaria por timestamp cumple su precondición.

## 3. Búsqueda por PM2.5

No se asume que los datos estén ordenados por PM2.5.

Por tanto, la búsqueda binaria por PM2.5 se conserva como experimento para demostrar el efecto de una precondición incumplida.

## 4. Comparación de String

Los identificadores de estación se comparan mediante:

```java
equals()
```

y no mediante:

```java
==
```

porque se necesita comparar contenido.

## 5. Medición

La comparación principal entre algoritmos utiliza el número de comparaciones.

El tiempo en milisegundos se conserva como evidencia experimental, pero no es la única medida utilizada.

## 6. Evolución del proyecto

La Semana 3 agrega una nueva capacidad a la misma plataforma:

```text
Sensores
   ↓
Ingesta
   ↓
Repositorio
   ↓
Búsqueda
   ↓
Medición de eficiencia
```



# Decisiones de diseño — Semana 3

## Búsqueda binaria por timestamp

### Precondición

La búsqueda binaria requiere que las lecturas estén ordenadas
ascendentemente por timestamp.

### Condición actual del proyecto

`GeneradorDatos` produce timestamps en orden cronológico.

### Decisión

Utilizar búsqueda binaria para consultas por timestamp.

### Justificación

La búsqueda binaria reduce el número de comparaciones
de un crecimiento O(n) a un crecimiento O(log n),
siempre que se mantenga la precondición de ordenamiento.

## PM2.5

No se utilizará directamente búsqueda binaria sobre PM2.5
mientras los datos no estén ordenados por ese campo.

El experimento de la Semana 3 demuestra la importancia
de respetar las precondiciones de un algoritmo.

## Pregunta pendiente

¿Conviene ordenar los datos antes de realizar las búsquedas?

Esta pregunta será retomada en la Semana 4.




La Semana 4 podrá extender esta misma arquitectura para estudiar ordenamiento.
