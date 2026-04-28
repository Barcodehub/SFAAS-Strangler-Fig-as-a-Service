# Módulo `diff-engine`

Propósito
--------
Librería pura (sin dependencias de Spring) que compara respuestas entre legacy y nuevo servicio. Debe ser fácilmente reutilizable desde el gateway, tests y cualquier módulo que necesite comparar payloads JSON/XML.

Dependencias principales
- `jackson-databind` (JSON)
- `xmlunit-core` (comparación XML)
- `junit-jupiter` / `assertj` para tests

Cómo usar
- Añadir dependencia a `project(':diff-engine')` y usar la API del paquete (por ejemplo: comparar dos JSON y obtener un resumen de diferencias con tolerancias configurables).

Ejecutar tests
```powershell
.\gradlew.bat :diff-engine:test
```

Relación con los 5 componentes
- Response Diff Engine — este módulo implementa el core de la funcionalidad de comparación.

Buenas prácticas
- Separar comparaciones sintácticas (orden de campos, whitespace) de comparaciones semánticas (valores críticos).
- Proveer modos de strict/lenient y configuraciones por endpoint.

