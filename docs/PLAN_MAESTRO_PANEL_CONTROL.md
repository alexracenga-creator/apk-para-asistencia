# Plan Maestro — Panel de Control de Asistencias

## Objetivo
Definir módulos, usuarios, sedes, reglas, mapas, GPS, asignaciones, estadísticas y reportes del Panel y su orden de construcción.

## Arquitectura
- APK Técnicos: crea y consulta solicitudes.
- APK Conductores: recibe/autoacepta solicitudes, ejecuta asistencias, registra GPS, almuerzo y actividades.
- Panel: usuarios, sedes, operación, asignaciones, mapa, GPS, vehículos, estadísticas y reportes.
- Clientes consumen Backend/API; no se conectan directamente a Neon.
- Neon PostgreSQL es la base central y debe poder migrarse posteriormente sin reconstruir APK ni Panel.

## Roles
- Administrador Nacional: usuarios, roles, permisos, sedes, vehículos, recursos, configuración, estadísticas e historial.
- Director/Gestor: alcance de su sede; solicitudes, asignaciones, mapa, GPS, almuerzos, actividades y estadísticas.

## Sedes
BQA-SOLEDAD, RIOHACHA, VALLEDUPAR, MEDELLÍN, BOGOTÁ, CARTAGENA y SANTA MARTA.
La sede administrativa no cambia automáticamente por GPS. El cambio de sede es administrativo y debe quedar en historial.

## Dashboard
NUEVAS, ASIGNADAS, EN_CAMINO, EN_ASISTENCIA, COMPLETADAS, CANCELADAS; conductores disponibles/en camino/en asistencia/en almuerzo; solicitudes pendientes y antigüedad; mapa operativo; asignación/reasignación rápida; última actualización GPS.

## Asistencias
Técnico, teléfono, sede, dirección, latitud/longitud, recurso, observación, creación, estado, conductor, tiempos de asignación/aceptación/salida/llegada/finalización e historial.
Recursos: Escalera de tijera; Escalera de extensión; Ambas; Auxiliar SENA; Herramientas; Otras.

## Asignación y autoaceptación
El Gestor puede asignar/reasignar. El conductor puede ver solicitudes disponibles de su sede y autoaceptar. La autoaceptación debe ser atómica para impedir doble asignación. Debe registrarse quién asignó, cuándo y observaciones. Se permite asignar durante ALMUERZO.

## Estados
NUEVA, ASIGNADA/ACCEPTED, EN_CAMINO, EN_ASISTENCIA, COMPLETADA, CANCELADA.

## Almuerzo y actividades
Cada conductor tiene una hora de almuerzo por jornada; se registra inicio/fin y el Panel muestra inicio, transcurrido, restante y hora estimada de fin. ALMUERZO no bloquea asignaciones.
Actividades: DISPONIBLE, ALMUERZO, EN_TRASLADO, MANTENIMIENTO_VEHICULO, CARGANDO_DESCARGANDO, TRAMITE_DILIGENCIA, ACTIVIDAD_ADMINISTRATIVA, ESPERANDO_INSTRUCCIONES, OTRA.

## GPS y mapa
GPS aproximadamente cada 4 segundos durante seguimiento. Guardar latitud, longitud, precisión, velocidad, rumbo, usuario, asistencia y fecha/hora. Mapa por sede para Gestor y vista nacional para Administrador. Las rutas históricas permiten calcular distancia y tiempos. Retención propuesta del GPS detallado: 3–6 meses, pendiente de definición final.

## Ruta
Conductor actual, posición actual, destino, ruta recorrida, distancia, tiempo de traslado, última posición y orden de asistencias.

## Usuarios y permisos
Crear/editar, activar/desactivar, rol/sede, cambio de sede con historial, credenciales, historial e importación Excel.
Roles: ADMIN, GESTOR, TECNICO, CONDUCTOR.
Permisos definidos en el plan: VER_PROPIA_ASISTENCIA, CREAR_ASISTENCIA, GESTIONAR_ASISTENCIAS, VER_MAPA_SEDE, VER_ESTADISTICAS_SEDE, GESTIONAR_ALMUERZOS, GESTIONAR_USUARIOS, GESTIONAR_SEDES, CAMBIAR_SEDE_USUARIO, IMPORTAR_USUARIOS_EXCEL, EXPORTAR_REPORTES, GESTIONAR_ROLES_PERMISOS.

## Vehículos
Placa única, tipo, marca/modelo, sede, activo/inactivo, observación e historial conductor–vehículo con fechas desde/hasta.

## Estadísticas y reportes
Filtros diario, semanal, 15 días, mensual, rango personalizado, conductor, todos los conductores y sede. Indicadores: asistencias, completadas/canceladas, kilómetros, tiempo de ruta, tiempo promedio, km promedio, última asistencia, estado actual y última actualización GPS. Exportación XLSX y reporte resumen/detallado.

## Historial y auditoría
Registrar creación, asignación/reasignación, aceptación, cambios de estado, cancelaciones/motivo, almuerzo, actividades, cambios de sede, importaciones, usuario responsable y fecha/hora.

## Seguridad
Login Backend/API, JWT, permisos por rol, restricción por sede, Administrador nacional, secretos solo en servidor. Nunca exponer credenciales de Neon en APK ni conectar APK directamente a PostgreSQL.

## Flujo extremo a extremo
Técnico inicia sesión → crea solicitud → Backend guarda → Gestor/conductor la ve → asignación/autoaceptación → conductor ejecuta → GPS → Panel/técnico reciben ubicación según permisos → EN_ASISTENCIA → COMPLETADA → historial/GPS/tiempos/estadísticas.

## Orden de construcción
1. Proyecto base e instalador Windows.
2. Login y roles.
3. Sedes y permisos.
4. Dashboard.
5. Gestión de asistencias.
6. Asignación y autoaceptación.
7. Mapa y GPS.
8. Conductores.
9. Almuerzos y actividades.
10. Usuarios y sedes.
11. Vehículos.
12. Historial/auditoría.
13. Estadísticas.
14. Exportación XLSX.
15. Configuración administrativa.
16. Pruebas integrales con ambas APK.
17. Instalador final sin Node/npm/Gradle/PostgreSQL en PCs de usuarios.

## Instalación final
Objetivo: Panel_Asistencias_Instalador.exe. En cada PC solo se instala el Panel; consume el Backend central.

## Criterio de cierre
Técnico → Backend → Panel/Gestor → Conductor → GPS → llegada → finalización → historial → estadísticas → XLSX, respetando sedes, permisos, almuerzo, actividades, vehículos y auditoría.

## Estado actual
Base de datos Neon: estructura definida y pruebas previas.
Backend/API: localizado en PC; configuración Neon en curso/validada durante el desarrollo.
APK Técnico: base funcional; pendiente prueba extremo a extremo.
APK Conductor: versión final encontrada; pendiente prueba real.
Panel de Control: POR CONSTRUIR; alcance definido en este documento.
Prueba extremo a extremo: pendiente después de estabilizar Backend/APKs.

> Este documento no contiene credenciales ni secretos.
