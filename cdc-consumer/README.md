# Módulo `cdc-consumer`

Propósito
--------
Consumidor CDC (Change Data Capture) que lee eventos (por ejemplo de Debezium publicados en Kafka) y sincroniza datos entre las bases de datos legacy y las nuevas. Además detecta conflictos y encola registros para resolución.

Dependencias principales
- `spring-kafka`
- `spring-boot-starter-data-jpa`
- `jackson-databind`
- `testcontainers-kafka` para pruebas

Cómo ejecutar
```powershell
.\gradlew.bat :cdc-consumer:bootRun
.\gradlew.bat :cdc-consumer:bootJar
```

Consideraciones de integración
- Requiere cluster Kafka para funcionar — en local puedes usar Testcontainers o un broker local.
- Debe integrarse con `core` para persistencia y con `admin-api` para notificaciones y gestión de conflictos.

Relación con los 5 componentes
- CDC Bidireccional con conflict resolution — este módulo implementa la ingesta y detección de conflictos.

