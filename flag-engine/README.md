# Módulo `flag-engine`

Propósito
--------
Proveer un servicio de feature flags con semántica específica para migraciones: reglas por módulo, tenant, usuario, porcentaje de tráfico y ventanas horarias. Estas flags ayudan a dirigir tráfico durante la migración y a probar progresivamente nuevos servicios.

Dependencias principales
- `spring-boot-starter-web`
- Librerías internas de `core` para modelos y repositorios

Cómo ejecutar
```powershell
.\gradlew.bat :flag-engine:bootRun
.\gradlew.bat :flag-engine:bootJar
```

Responsabilidades típicas
- Exponer API para evaluar reglas de flags (p. ej. `isEnabled(module, tenant, userId)`)
- Interfaz para CRUD de flags (ideally protegida vía `admin-api`)
- Integración con `gateway` para tomar decisiones en tiempo real

Relación con los 5 componentes
- Feature Flags con semántica de migración — implementación principal aquí.

