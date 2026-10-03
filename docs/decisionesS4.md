## DEC-04 — Pivote de QuickSort

**Semana:** 4

**Problema:**
QuickSort con pivote fijo (primer elemento) produce particiones
extremadamente desbalanceadas cuando los datos ya vienen ordenados.
Como las lecturas de la red llegan en orden cronológico (por timestamp),
este es justamente nuestro caso real, no un caso raro. En lugar de dividir
el problema en dos mitades, deja un lado vacío y otro casi completo, lo que
convierte la recursión en una cadena de profundidad n y agota la pila.

**Alternativas:**
- Pivote aleatorio.
- Mediana de tres (primero, medio, último).

**Decisión:**
Se eligió el **pivote aleatorio**: antes de particionar se selecciona una
posición al azar dentro del rango y se intercambia con el primer elemento.

**Justificación (con nuestras mediciones):**
- Pivote fijo, 50.000 lecturas ORDENADAS: StackOverflowError, alcanzando
  657.348.622 comparaciones antes de quedarse sin pila.
- Pivote aleatorio, 50.000 lecturas ORDENADAS: 979.722 comparaciones,
  546.218 intercambios, 377 ms (termina correctamente).
- Pivote aleatorio, 50.000 lecturas DESORDENADAS: ~900.000 comparaciones,
  comportamiento equivalente al caso ordenado.
El pivote aleatorio elimina la dependencia del orden de entrada: ya no existe
un patrón de datos que fuerce el peor caso de forma sistemática.

**Consecuencia:**
QuickSort deja de depender de cómo lleguen los datos y se comporta de forma
estable incluso con la entrada cronológica real de la red. Se añade un costo
mínimo y se introduce aleatoriedad, por lo que el número exacto de comparaciones varía
ligeramente entre ejecuciones.

## DEC-05 — Ordenamiento y búsqueda (efecto colateral)

**Semana:** 4

**Problema:**
Ordenar las lecturas por PM2.5 para construir el ranking destruye el orden
por timestamp. Como la búsqueda binaria por timestamp exige que los datos
estén ordenados por ese mismo criterio, tras el ranking la búsqueda binaria
deja de funcionar, aunque la lectura siga existiendo.

**Alternativas:**
- Trabajar sobre una copia.
- Restaurar el orden por timestamp después del ranking.
- Mantener índices separados por cada criterio.

**Decisión:**
Para el ranking se trabaja sobre una **copia** del arreglo; el arreglo
principal se mantiene siempre ordenado por timestamp.

**Justificación (con nuestras mediciones):**
En el Experimento 5 lo comprobamos: con los datos ordenados por timestamp,
la búsqueda binaria encontró la lectura en la posición 73412 con solo 16
comparaciones. Tras ordenar por PM2.5, "ordenado por timestamp" pasó a false
y la búsqueda binaria devolvió -1 (falla), mientras la búsqueda lineal sí la
encontró (posición 87705). La lectura nunca desapareció; lo que se rompió fue
la precondición de la binaria. Como la consulta por timestamp es frecuente y
el ranking es ocasional, conviene no sacrificar el orden principal. La copia
cuesta memoria O(n) y un recorrido extra, costo aceptable para preservar la
consulta rápida.

**Consecuencia:**
El ranking consume memoria adicional (una copia) y un paso de copiado, pero
la búsqueda binaria por timestamp sigue siendo válida en todo momento. Si en
el futuro se necesitan consultas rápidas por varios criterios a la vez, se
migraría a la estrategia de índices separados.
