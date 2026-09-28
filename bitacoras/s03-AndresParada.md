# Bitacora individual - Semana [3]


## 1. Datos de la actividad

- **Estudiante:** Andres Felipe Parada Orozco
- **Equipo:** 1
- **Semana:** 28/09/2026
- **Fecha del laboratorio:** 26/09/2026
- **Fecha del taller:** 27/09/2026
- **Tema principal:** Algoritmos de Búsqueda Lineal y Binaria y análisis de eficiencia en arreglos.
- **Pregunta de la semana:** ¿Cómo encontramos una lectura específica eficientemente cuando el repositorio escala a millones de registros?

## 2. Prediccion antes de ejecutar

Antes de abrir o ejecutar el programa, responde:

1. **Que creo que va a ocurrir?**
   Creo que al buscar la última lectura, la búsqueda lineal tardará mucho por que va a recorrer cada elemento arreglo, mientras que la búsqueda binaria será mucho más rápida y efectiva gracias a su comportamiento logarítmico

2. **Que parte del programa o del algoritmo puede fallar?**
   La búsqueda binaria puede dar datos que parecen verdadedros pero son falsos si los datos no están debidamente ordenados
3. **Como comprobare mi prediccion?**
    Al poder revisar la cantidad de comparaciones devueltas al buscar datos que nunca han estado o existido

## 3. Evidencia del laboratorio

### Resultado observado

Al buscar el peor caso en 1,000,000 de registros, la búsqueda lineal requirió 1,000,000 de comparaciones. La búsqueda binaria halló el mismo dato mucho más rápido. En el Experimento 4 con PM2.5 desordenado, la búsqueda binaria falló y no encontró 18 de los 20 valores que sí existían pero no estaba ordenados

### Diferencia entre la prediccion y el resultado

La predicción fue acertada. Se comprobó que el número de comparaciones en la búsqueda lineal crece proporcionalmente con el tamaño de los datos mientras que la binaria fue mucho más rápida.

### Error o comportamiento inesperado

- **Que ocurrio?** La búsqueda binaria arrojó falsos negativos en PM2.5
- **Por que ocurrio?** Descartaba bloques enteros del arreglo asumiendo que estaban ordenados
  - **Como lo corregimos o que falta corregir?** Para corregirlo, es obligatorio cumplir la precondición del algoritmo: ordenar el arreglo por PM2.5 antes de realizar la búsqueda binaria.

## 4. Explicacion en lenguaje llano

Explica el concepto principal como se lo explicarias a una persona de doce
anos. Usa entre tres y cinco lineas y evita palabras tecnicas que no expliques.

La búsqueda lineal es como revisar página por página desde el inicio hasta el final de un diccionario para buscar una palabra; si la palabra no existe, terminarás y habrás gastado mucho tiempo. La búsqueda binaria es abrir el diccionario en la mitad, ver si tu palabra está antes o después alfabéticamente, y arrancar desde la mitad donde si va a estar, repitiendo el proceso hasta encontrarla en menor tiempo.

### Ejemplo o analogia

Es como buscar un número de teléfono en la guía telefónica. Cada consulta nos permite descartar la mitad del libro restante. Deja de ser exacta porque en la vida real no abrimos el libro matemáticamente en la mitad perfecta en cada intento.

## 5. El vacio que encontre

Al intentar explicar el tema, identifica el punto que aun no comprendes bien.

- **Mi duda concreta es:** ¿Vale la pena pagar el costo de ordenar los datos con un algoritmo de ordenamiento solo para poder usar la búsqueda binaria después?
- **Lo que ya puedo explicar es:** Que la búsqueda binaria es exponencialmente superior a la lineal, pero es inútil si el arreglo no está ordenado.
- **Para resolver la duda consulte:** Otra fuente
- **Ahora lo entiendo así:** Si algo se busca pocas veces, conviene la lineal; si se busca constantemente, conviene ordenar una sola vez para usar la binaria.

## 6. Trazado de la solucion

Escoge una ejecucion, recorrido o caso representativo y trazalo paso a paso.
Incluye los valores importantes despues de cada paso.

| Paso | Estado de los datos o estructura | Decision o resultado |
|---|---|---|
| 1 | inicio=0, fin=3, medio=1, valor=1 | 1 < 3 (Buscamos a la derecha): inicio = medio + 1 |
| 2 | inicio=1, fin=3, medio=2, valor=2| 2 < 3 (Buscamos a la derecha): inicio = medio + 1 |
| 3 | inicio=2, fin=3, medio=2, valor=2 | 3 == 3. ¡Elemento encontrado! Retorna índice 3. |
| 4 | inicio=2, fin=3, medio=2, valor=2| Los valores no cambian. El ciclo while(inicio <= fin) se ejecuta de forma infinita |

**Completa o agrega filas si es necesario.** Si trabajaste con una estructura,
dibuja su estado en cada paso o inserta aqui una imagen legible.

## 7. Decision de diseño

Relaciona lo aprendido con la Plataforma de Monitoreo Ambiental Urbano.

- **Problema que debiamos resolver:** Encontrar lecturas específicas en una red con millones de registros
- **Estructura, algoritmo o estrategia elegida:** Búsqueda binaria para consultas por timestamp.
- **Alternativa descartada:**  Búsqueda lineal por timestamp o búsqueda binaria directa
- **Por que elegimos la primera:** La binaria por timestamp reduce las operaciones de forma segura. Se descartó en PM2.5 porque los datos son aleatorios y romperían el algoritmo.
- **Que evidencia respalda la decision:** Las mediciones del Experimento 2 y los fallos del Experimento 4.

## 8. Aporte al proyecto

- **Archivo(s) o modulo(s) trabajado(s):** src/BuscadorLecturas.java, src/GeneradorDatos.java, src/BancoDePruebas.java.
- **Cambio realizado:** Implementación de algoritmos de búsqueda lineal y binaria e instrumentación del banco de experimentos
- **Como se conecta con la capa anterior:** Consume los objetos LecturaSensor validados por la capa de ingesta de la Semana 1 y 2 para buscar sobre ellos en memoria continua.
- **Que queda pendiente para la siguiente semana:** Implementar algoritmos de ordenamiento en la Semana 4 para habilitar búsquedas complejas


## 9. Commits realizados

Registra los commits que muestran tu aporte individual.

| Commit | Mensaje | Que demuestra |
|---|---|---|
| `c06f8ac` | `feat: implement BuscadorLecturas con busqueda lineal y binaria` | Implementación del algoritmo principal optimizado. |
| `58bc21c` | `test: agregar experimentos dos y tres para comparar eficiencia lineal vs binaria` | Inclusión de métricas y evaluación comparativa en el banco de pruebas. |
| `1c48258` | `feat: implementar experimento cuatro para evaluar impacto de precondicion PM2.5` | Demostración del fallo del algoritmo ante datos desordenados. |

## 10. Reexplicacion final

Despues del taller, vuelve a responder la pregunta de la semana en cinco lineas
o menos. Esta respuesta debe ser mas precisa que la de la seccion 4 y debe
incluir la razon de tu decision tecnica.

> Para encontrar un dato entre millones de registros de forma eficiente, se utiliza la búsqueda binaria, la cual descarta la mitad del arreglo en cada iteración reduciendo el costo y tiempo. Sin embargo, esta estrategia tiene una condición: exige estrictamente que los datos estén ordenados por el campo a buscar o sino fallará silenciosamente todo. 

## 11. Reflexion individual

Responde con honestidad:

1. **Lo que ahora puedo hacer y antes no podia:**
   Ahora puedo crear un banco de pruebas experimental para medir y contrastar la eficiencia temporal y operacional de los algoritmos
2. **El error o supuesto que mas me enseno:**
   El experimento con PM2.5. Me demostró que un algoritmo puede estar impecablemente codificado, pero si se ignoran las precondiciones de los datos de entrada, el sistema producirá fallos invisibles
3. **La pregunta que llevaria a la proxima clase:**
   Por ahora no tengo
4. **Que parte del trabajo fue realmente mia:**
   El análisis de los resultados lógicos de las precondiciones, la depuración manual del archivo BuscadorLecturas.java para sincronizarlo con los experimentos y la organización secuencial del historial web de Git.

## Lista de verificacion antes de entregar

- [ ] Escribi la prediccion antes de consultar el resultado.
- [ ] Inclui evidencia concreta del laboratorio.
- [ ] Explique un concepto sin depender de jerga.
- [ ] Registre un vacio, una duda o un error real.
- [ ] Trace al menos un caso paso a paso.
- [ ] Justifique una decision del proyecto y una alternativa descartada.
- [ ] Registre mis commits y mi aporte individual.
- [ ] Deje claro que queda pendiente.
- [ ] Renombre el archivo con el formato `sXX-nombre.md`.
