# Estado de continuidad — 8 de octubre de 2026

## Objetivo
Sistema de asistencias técnicas compuesto por APK Técnico, APK Conductor y Panel de Control de escritorio Windows. Los clientes usan el Backend/API y Neon PostgreSQL; ninguna APK ni el Panel se conecta directamente a Neon.

## Reglas confirmadas
- Una sola APK de técnicos y una sola APK de conductores para todas las sedes.
- El inicio de sesión es por cédula. El backend debe resolver identidad, rol, sede, estado y permisos; el cliente no decide ni puede falsificar su sede.
- La sede del usuario se administra centralmente desde el Panel. Solicitudes y consultas quedan limitadas por rol/permisos/sede en el backend.
- El técnico crea una solicitud con recurso, dirección/ubicación y observación. La misma solicitud conserva su identificador durante todo el ciclo.
- Solo tras confirmar el guardado, mostrar «Solicitud enviada correctamente» y limpiar el formulario. Si falla, conservar datos y mostrar el error real.
- GPS y envío de solicitudes son independientes. El GPS no debe bloquear el botón ni reiniciar el formulario.
- Mantener el GPS de la APK técnica en 5 segundos durante la corrección inmediata; no mostrar conteo repetitivo en el menú.
- La solicitud aparece en «Mis asistencias» desde que se crea, inicialmente sin conductor asignado.
- El gestor puede asignar/reasignar. El conductor puede autoaceptar solicitudes disponibles de su sede cuando esa función esté integrada. La autoaceptación debe impedir doble asignación.
- El Gestor puede asignar durante ALMUERZO, según el plan.
- El Panel de Windows debe ser instalable como aplicación normal; el usuario final no debe necesitar abrir CMD ni instalar Java/Node/Gradle manualmente.
- No rehacer interfaces aprobadas ni modificar componentes que funcionan sin evidencia concreta.
- No guardar credenciales ni secretos en GitHub.

## Estado más reciente reportado por el usuario desde otra conversación
Este es el último informe compartido por el usuario; refleja el estado reportado allí y no significa que todos los puntos se hayan vuelto a verificar en esta sesión.

### Bloque 0 — Organización y continuidad
- Proyecto apk para asistencia.
- Mantener una ruta fija, un bloque a la vez, con prueba verificable y estado documentado.

### Bloque 1 — Base de datos Neon
Reportado completado:
- Neon conectado y estructura principal creada.
- Usuarios, roles, sedes, recursos, asistencias, asignaciones, GPS, almuerzos, actividades, vehículos y permisos.
- Pruebas con varios conductores.

### Bloque 2 — Backend/API
Reportado completado en las pruebas anteriores:
- Login, creación de asistencia, asignación/aceptación, estados, GPS, almuerzo, actividades y prueba de asistencia, incluyendo prueba con tres conductores.
- En otro informe más reciente, se indicó que la corrección del endpoint de estados estaba aplicada y compilada; quedaba confirmar EN_CAMINO después de reiniciar el backend.
- Se reportó también que la creación de asistencia real en Neon, la visualización de solicitudes disponibles y el registro de la asignación en asistencia_asignaciones funcionaban.
- Debe desplegarse el Backend/API fuera del PC del usuario para que el sistema siga funcionando cuando el PC se apague.

### Bloque 3 — APK para Técnicos
Reportado como base funcional:
- Diseño, recursos, GPS y APK instalable/firmada.
- Falta consolidar/verificar la conexión definitiva al backend central.
- Hubo una versión modificada que se quedaba en «Enviando solicitud…» y mostraba «No se puede reenviar la solicitud de asistencia». La causa no quedó confirmada en el informe anterior. Comparar el código actual con la versión funcional antes de editar.
- Preservar la interfaz aprobada y separar GPS del envío de solicitudes.

### Bloque 4 — APK para Conductores
Reportado como APK funcional y firmada; login probado.
- Se reportó que el conductor veía asistencias disponibles y que la autoaceptación funcionaba en pruebas del backend, con asignación registrada en asistencia_asignaciones.
- El último estado general indica que aún falta consolidar la conexión real al backend central y la integración de la aceptación de asistencias disponibles en la APK. Distinguir pruebas de endpoints/backend de la integración dentro de la APK.
- Falta prueba integral de estados, GPS y cierre.

### Bloque 5 — Panel de Control Windows
- Pendiente de verificar y completar. Una conversación anterior mencionó Panel_Control_Bloque_5_v1.zip y una estructura inicial con login, tablero, asistencias, conductores, técnicos, mapa, estadísticas y administración; no hay evidencia en este estado de que el ZIP esté disponible en el repositorio ni de que compile o esté conectado a datos reales. No afirmar que el panel está construido/validado hasta inspeccionar los archivos reales.
- Debe conectarse a la API existente, nunca directamente a Neon.
- Funciones planificadas: login ADMIN/GESTOR, dashboard por estados, mapa/GPS, solicitudes y asignaciones, conductores, técnicos, almuerzos/actividades, estadísticas, reportes XLSX, usuarios, roles, permisos, sedes, recursos, vehículos e importación Excel.
- Instalador objetivo: Panel_Asistencias_Instalador.exe.

### Bloque 6 — Integración completa
Pendiente de una prueba integral real:
Técnico crea → Backend guarda → Panel ve/asigna o conductor autoacepta → conductor EN_CAMINO → GPS → llegada → EN_ASISTENCIA → COMPLETADA → historial y estadísticas.

### Bloque 7 — Producción
Pendiente:
- Desplegar Backend/API en servidor central.
- Configuración segura y permisos por rol/sede.
- Retirar mecanismos DEMO después de validar el circuito real.
- Probar con varios teléfonos.
- Preparar instalador del Panel y verificar APK firmadas.
- Prueba completa de producción.

## Ruta de trabajo
1. No repetir pruebas aprobadas de Neon/backend sin una causa concreta.
2. Inspeccionar el repositorio y los archivos reales antes de elegir la siguiente modificación.
3. Confirmar el endpoint de estado EN_CAMINO tras el reinicio si aún no se ha hecho.
4. Verificar y consolidar el envío de la APK técnica sin perder el código/interfaz funcional.
5. Consolidar la integración de la APK de conductores con la API: solicitudes disponibles, aceptación, estados y GPS.
6. Inspeccionar si existe realmente el proyecto Panel Bloque 5 v1; si existe, revisarlo y conectarlo; si no, construir el panel mínimo sobre endpoints reales.
7. Hacer la prueba integral; después completar GPS de ambos perfiles, módulos restantes del Panel y despliegue central.
8. Registrar en este documento cada cambio solo después de que su prueba tenga un resultado verificable.

## Planes maestros
- docs/PLAN_MAESTRO_PANEL_CONTROL.md
- docs/PLAN_MAESTRO_SISTEMA_ASISTENCIAS.md

## Repositorio
alexracenga-creator/apk-para-asistencia