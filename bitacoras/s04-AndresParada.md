# Bitacora individual - Semana 04

## 1. Datos de la actividad

- **Estudiante:** Andrés Felipe Parada Orozco
- **Equipo:** 1
- **Semana:** 4
- **Fecha del laboratorio:** 10/3/2026
- **Fecha del taller:** 10/3/2026
- **Tema principal:** Algoritmos de ordenamiento (Burbuja, Selección, Inserción, MergeSort, QuickSort y HeapSort) y comparación de eficiencia.
- **Pregunta de la semana:** Si ordenar es necesario para poder buscar rápido, ¿cuánto cuesta ordenar y qué consecuencias tiene hacerlo sobre el resto del sistema?

## 2. Prediccion antes de ejecutar

1. **Que creo que va a ocurrir?**
   Yo pensaba que los seis algoritmos iban a terminar dando lo mismo  y que la única diferencia iba a ser que unos tardaran un poco más que otros. Esperaba que los avanzados fueran claramente más rápidos que los simples cuando subiera la cantidad de datos, sobre todo con 100.000 lecturas.

2. **Que parte del programa o del algoritmo puede fallar?**
   Me daba desconfianza el QuickSort con el pivote en el primer elemento. No tenía claro por qué, pero quería comprobar qué pasaba cuando le metiera los datos de la red que ya vienen ordenados por timestamp.

3. **Como comprobare mi prediccion?**
   Corriendo los cinco experimentos del banco y mirando las tres métricas de cada algoritmo: comparaciones, intercambios y tiempo. Y en el experimento 4 iba a comparar el QuickSort con 50.000 datos desordenados contra 50.000 datos ya ordenados.

## 3. Evidencia del laboratorio

### Resultado observado

Corrí todo desde `IngestaSensores` y salieron los cinco experimentos. Lo más claro:

- Con 10.000 datos desordenados, Burbuja hizo 49.990.814 comparaciones y 24.928.244 intercambios; Selección hizo las mismas 50 millones de comparaciones pero solo 9.994 intercambios; Inserción hizo 24.938.233 comparaciones.
- Con 10.000 datos ya ordenados, Burbuja (con mi bandera) bajó a 9.999 comparaciones y 1 ms. Selección se quedó igual en 49.995.000 porque no tiene bandera.
- Subiendo n de 1.000 a 10.000 a 100.000: Inserción se multiplicó por ~100 cada vez (llegó a 2.497.222.762 comparaciones con 100.000) y MergeSort/HeapSort se multiplicaron por ~13.
- QuickSort con pivote fijo: con 50.000 desordenados funcionó bien, pero con 50.000 ordenados reventó con StackOverflowError, alcanzando 657.348.622 comparaciones antes de morir. Con pivote aleatorio ordenó esos mismos datos en 979.722 comparaciones y 377 ms.
- En el experimento 5, la búsqueda binaria por timestamp encontró la lectura (posición 73412, 16 comparaciones). Después de ordenar por PM2.5 devolvió -1, pero la lineal sí la encontró (posición 87705).

### Diferencia entre la prediccion y el resultado

Acerté en que los avanzados escalan muchísimo mejor. En lo que me equivoqué fue en pensar que todos terminaban igual: el QuickSort con pivote fijo ni siquiera terminó con los datos reales, se cayó. Eso no me lo esperaba tan grave.

### Error o comportamiento inesperado

- **Que ocurrio?** El QuickSort con el primer elemento como pivote explotó con StackOverflowError justo con los datos que llegan ordenados de la red.
- **Por que ocurrio?** Cuando los datos ya están ordenados y el pivote siempre es el primero, la partición deja un lado vacío y el otro con casi todo. En vez de dividir el problema en dos mitades, lo va bajando de uno en uno, y la recursión llega a profundidad n hasta que se acaba la pila.
- **Como lo corregimos o que falta corregir?** Cambié el pivote a aleatorio: antes de particionar escojo una posición al azar y la intercambio con la primera. Así ya no hay un patrón de datos que fuerce siempre el peor caso, y los 50.000 ordenados sí terminan bien.

## 4. Explicacion en lenguaje llano

> Ordenar es poner las cosas de menor a mayor. Todos estos métodos hacen lo mismo, pero piensan distinto: unos comparan parejas de vecinos, otros buscan el más pequeño y lo ponen adelante, otros parten el montón en pedacitos y los van juntando. Lo importante no es solo cuál termina, sino cuánto trabajo le cuesta y si los datos que le das le ayudan o lo hacen sufrir.

### Ejemplo o analogia

Es como ordenar un grupo de personas por estatura. Burbuja va de dos en dos pidiéndoles que se cambien de puesto si están al revés; Selección mira a todos y saca al más bajito para ponerlo de primero; Inserción toma a uno nuevo y lo mete en su lugar como cuando organizas cartas en la mano. La analogía deja de ser exacta cuando llegas a MergeSort o HeapSort, porque ahí ya no es "mirar la fila": es partir el grupo en subgrupos o armar una estructura especial, que es más difícil de imaginar con personas en una fila.

## 5. El vacio que encontre

- **Mi duda concreta es:** ¿Por qué exactamente el pivote fijo hace que la recursión llegue a profundidad n y se acabe la pila, y por qué el aleatorio lo evita?
- **Lo que ya puedo explicar es:** Que con datos ordenados y pivote fijo las particiones quedan muy desbalanceadas (un lado vacío, otro casi completo).
- **Para resolver la duda consulte:** La guía de la semana, el experimento 4 corriéndolo yo mismo (primero con el crash y luego con el pivote aleatorio) y las explicaciones del taller.
- **Ahora lo entiendo asi:** Cada vez que QuickSort parte, se llama a sí mismo sobre los dos lados. Si un lado siempre queda casi completo, esas llamadas se van apilando una encima de otra n veces sin cerrarse, y la pila de ejecución se llena. El pivote aleatorio rompe ese patrón porque casi nunca le toca justo el extremo, así que los dos lados quedan más parecidos y la recursión no se hace tan profunda.

## 6. Trazado de la solucion

Tracé QuickSort con pivote fijo (primer elemento) sobre un arreglo pequeño YA ordenado `[1, 2, 3, 4, 5]`, para ver por qué se degrada:

| Paso | Estado de los datos o estructura | Decision o resultado |
|---|---|---|
| 1 | `[1, 2, 3, 4, 5]`, pivote = 1 | Nadie es menor que 1 → el lado izquierdo queda vacío y el derecho `[2,3,4,5]` |
| 2 | `[2, 3, 4, 5]`, pivote = 2 | Izquierda vacía otra vez → derecha `[3,4,5]` |
| 3 | `[3, 4, 5]`, pivote = 3 | Izquierda vacía → derecha `[4,5]` |
| 4 | `[4, 5]`, pivote = 4 | Izquierda vacía → derecha `[5]` |
| 5 | `[5]` | Caso base, pero ya se hicieron n niveles de recursión apilados |

Con solo 5 datos no pasa nada, pero con 50.000 esos "n niveles apilados" son los que llenan la pila y producen el StackOverflowError.

## 7. Decision de diseño

- **Problema que debiamos resolver:** QuickSort se caía con los datos reales de la red (que llegan ordenados por timestamp) por culpa del pivote fijo.
- **Estructura, algoritmo o estrategia elegida:** QuickSort con pivote aleatorio.
- **Alternativa descartada:** Mediana de tres (primero, medio, último).
- **Por que elegimos la primera:** El pivote aleatorio era más simple de implementar y ya resolvía el problema de raíz: quita la dependencia del orden de entrada. El costo extra es mínimo (una llamada a Math.random y un intercambio por partición).
- **Que evidencia respalda la decision:** Con pivote fijo y 50.000 datos ordenados hubo StackOverflowError (657.348.622 comparaciones antes de caerse); con pivote aleatorio esos mismos datos se ordenaron en 979.722 comparaciones y 377 ms.

## 8. Aporte al proyecto

- **Archivo(s) o modulo(s) trabajado(s):** `src/Ordenador.java`, `src/BancoDeOrdenamiento.java` (nuevo) y `src/IngestaSensores.java`.
- **Cambio realizado:** Completé los seis algoritmos, agregué la bandera de corte temprano en Burbuja (TODO 1) y el pivote aleatorio en QuickSort (TODO 2). Creé el banco con los cinco experimentos y lo conecté al `main`.
- **Como se conecta con la capa anterior:** El experimento 5 usa la búsqueda binaria de la Semana 3 (`BuscadorLecturas`) para demostrar que ordenar por PM2.5 rompe la búsqueda por timestamp. Todo sigue entrando por el único `main` de `IngestaSensores`, igual que los experimentos de la Semana 3.
- **Que queda pendiente para la siguiente semana:** Decidir e implementar bien la estrategia para el conflicto de ordenar por varios criterios (por ahora quedó la idea de trabajar sobre una copia; más adelante se podrían usar índices separados). La estructura interna del montículo de HeapSort la construiremos en la Semana 6.

## 9. Commits realizados

| Commit | Mensaje | Que demuestra |
|---|---|---|
| `c3f1fde` | `feat: implementar ordenamientos simples` | Burbuja, Selección e Inserción |
| `35a3d28` | `fix: agregar corte temprano a burbuja` | TODO 1: la bandera de Burbuja |
| `b0c9ebc` | `feat: implementar ordenamientos avanzados` | MergeSort, HeapSort y QuickSort |
| `07af0a4` | `feat: Cambiar el Pivote para QuickSort` | TODO 2: pivote aleatorio |
| `668e738` | `feat: agregar BancoDeOrdenamiento con los 5 experimentos e integrarlo a IngestaSensores` | Banco de experimentos integrado al único main |
| `006a1b0` | `docs: registrar DEC-04 y DEC-05 y grafica semana 4` | Decisiones y evidencia |
| `9b45d18` | `docs: registrar Bitacora semana 4` | Bitácora individual |

## 10. Reexplicacion final

> Ordenar no es gratis: los algoritmos simples cuestan O(n²) y los avanzados O(n log n), y eso se nota cuando crecen los datos (Inserción se multiplicó por 100 y MergeSort por 13 al subir n). Pero el costo no depende solo del algoritmo, sino también de cómo llegan los datos: con datos ordenados, Inserción vuela y QuickSort con pivote fijo se cae. Y hay una consecuencia extra: ordenar por un criterio (PM2.5) puede romper una operación que dependía de otro orden (la búsqueda binaria por timestamp). Por eso elegí pivote aleatorio y trabajar el ranking sobre una copia.

## 11. Reflexion individual

1. **Lo que ahora puedo hacer y antes no podia:**
   Antes solo sabía "ordenar". Ahora puedo mirar un algoritmo y explicar por qué se comporta como se comporta según los datos, y defender con números cuál conviene.
2. **El error o supuesto que mas me enseno:**
   Creer que todos los algoritmos "terminan igual". El StackOverflowError de QuickSort me enseñó que una buena media no te salva del peor caso.
3. **La pregunta que llevaria a la proxima clase:**
   ¿Cómo se implementan en la práctica los índices separados para poder buscar rápido por timestamp y por PM2.5 al mismo tiempo sin tener que reordenar?
4. **Que parte del trabajo fue realmente mia:**
   La implementación de la bandera y el pivote aleatorio, armar y correr el banco de experimentos, capturar las mediciones y redactar las decisiones DEC-04 y DEC-05.

## Lista de verificacion antes de entregar

- [x] Escribi la prediccion antes de consultar el resultado.
- [x] Inclui evidencia concreta del laboratorio.
- [x] Explique un concepto sin depender de jerga.
- [x] Registre un vacio, una duda o un error real.
- [x] Trace al menos un caso paso a paso.
- [x] Justifique una decision del proyecto y una alternativa descartada.
- [ ] Registre mis commits y mi aporte individual. (faltan los hash cortos)
- [x] Deje claro que queda pendiente.
- [x] Renombre el archivo con el formato `sXX-nombre.md`.