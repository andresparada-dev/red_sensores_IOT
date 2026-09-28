# Traza de Búsqueda Binaria (Ciclo Infinito)

**Arreglo:** [0, 1, 2, 3]
**Dato buscado:** 3

| Paso | inicio | fin | medio | valor medio | acción (si no se corrige) |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 3 | 1 | 1 | inicio = medio (en lugar de +1) |
| 2 | 1 | 3 | 2 | 2 | inicio = medio (en lugar de +1) |
| 3 | 2 | 3 | 2 | 2 | inicio = medio (se queda atascado) |
| 4 | 2 | 3 | 2 | 2 | ciclo infinito |
