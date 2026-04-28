# Adaptadores (`core/adapters`)

Propósito
--------
Contiene las implementaciones concretas de las interfaces definidas en `core.ports`. Cada adaptador debe agruparse por tecnología (postgres, mysql, mongodb) y añadir sólo las dependencias que necesita (JPA, R2DBC, Mongo driver, etc.).

Estructura recomendada
- `core/adapters/postgres` — implementaciones basadas en Postgres (JPA o R2DBC)
- `core/adapters/mysql` — implementaciones para MySQL
- `core/adapters/mongodb` — implementaciones para MongoDB

Cómo añadir un adaptador
1. Crear la carpeta del adaptador (ya existe la estructura básica).
2. Añadir las implementaciones que implementen las interfaces en `core.ports`.
3. Declarar dependencias en el `build.gradle` del adaptador (drivers, spring-data, etc.).

Ejemplo rápido (pseudocódigo)
```java
@Repository
public class PostgresRouteRepository implements RouteRepository {
    // implementación usando Spring Data JPA o R2DBC
}
```

Consideraciones
- Mantener la lógica de adaptación (mapeos, conversiones) dentro del adaptador, no en `core`.
- Los adaptadores pueden depender de Spring Boot en módulos específicos (ej.: `admin-api` o módulos runtime) pero `core` debe permanecer ligero.

