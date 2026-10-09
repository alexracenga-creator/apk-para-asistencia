# Ruta maestra de trabajo — APK para asistencias

**Repositorio:** `alexracenga-creator/apk-para-asistencia`  
**Documento vivo:** esta ruta es el plan operativo principal. Actualizar su estado solo después de una prueba con resultado verificable.  
**Punto de partida:** fase de APK; prioridad inmediata: APK 0 del técnico y el envío de solicitudes.

## 1. Objetivo del sistema

Sistema compuesto por:
- APK para técnicos.
- APK para conductores.
- Panel de control instalable para Windows.
- Backend/API central y Neon PostgreSQL.

Las APK y el Panel se comunican con Neon únicamente a través del Backend/API; ningún cliente se conecta directamente a la base de datos.

## 2. Reglas y decisiones aprobadas

- Una APK de técnicos y una APK de conductores para todas las sedes.
- Inicio de sesión por cédula. El backend determina identidad, rol, sede, estado y permisos; no confiar en un `site_id` enviado por el cliente.
- La sede y los permisos se administran centralmente desde el Panel.
- Mantener el mismo identificador de asistencia durante todo su ciclo.
- La solicitud debe aparecer en «Mis asistencias» desde su creación, inicialmente sin conductor asignado.
- Mostrar «Solicitud enviada correctamente» y limpiar el formulario solo después de confirmar el guardado. Si falla, conservar los datos y mostrar el error real.
- El GPS y el envío de solicitudes son independientes. El GPS no debe bloquear el botón ni reiniciar el formulario.
- Mantener el GPS de la APK técnica a 5 segundos durante la corrección inmediata; no mostrar un conteo repetitivo en el menú.
- El gestor puede asignar o reasignar solicitudes, incluso durante ALMUERZO según el plan.
- La autoaceptación del conductor debe ser atómica para impedir que dos conductores acepten la misma asistencia.
- El Panel debe instalarse como aplicación normal; el usuario final no debe necesitar abrir CMD ni instalar Java/Node/Gradle manualmente.
- Preservar las interfaces aprobadas. No rediseñar ni modificar componentes que funcionan sin una causa concreta.
- No guardar credenciales ni secretos en GitHub.
- No repetir pruebas satisfactorias de Neon/backend por rutina. Distinguir siempre prueba de endpoint, prueba desde APK y prueba integral.
- No marcar una fase como terminada hasta comprobar su criterio de cierre y registrar el resultado.

## 3. Estado de referencia informado por el usuario

Este estado procede de informes de conversaciones anteriores; no implica que todos los puntos se hayan vuelto a ejecutar o verificar en la sesión actual.

### Bloque 0 — Organización y continuidad
**Estado informado: completado.** Se estableció la ruta de trabajo, la documentación y la regla de avanzar por fases con pruebas verificables.

### Bloque 1 — Base de datos Neon
**Estado informado: completado.** Se reportaron conexión y tablas para usuarios, roles, sedes, recursos, asistencias, asignaciones, GPS, almuerzos, actividades, vehículos y permisos; también pruebas con varios conductores.

### Bloque 2 — Backend/API
**Estado informado: funciones principales probadas.** Se reportaron pruebas de login, creación de asistencia, solicitudes disponibles, asignación/aceptación y otras funciones mediante PowerShell y registros en Neon. En un informe anterior quedó pendiente confirmar el estado EN_CAMINO después de reiniciar el backend. No repetir pruebas ya aprobadas salvo que la integración de la APK o un fallo concreto lo requiera.

### Bloque 3 — APK del técnico
**Estado: trabajo actual; cierre pendiente.**
- Existe una base funcional con interfaz aprobada, recursos, GPS y APK instalable/firmada según el informe previo.
- Una versión modificada se queda en «Enviando solicitud…» y muestra «No se puede reenviar la solicitud de asistencia».
- La causa no está confirmada. Comparar el código actual con la versión funcional antes de editar.
- Prioridad: diagnosticar y corregir el envío, sin rediseñar la interfaz ni acoplar el GPS al envío.

### Bloque 4 — APK del conductor
**Estado: base funcional reportada; integración completa por confirmar.**
- Se reportaron login probado y solicitudes disponibles/autoaceptación en pruebas del backend.
- Falta comprobar desde la APK real que consulta solicitudes, acepta/asigna, actualiza estados y transmite GPS mediante la API central.

### Bloque 5 — Panel de control Windows
**Estado: pendiente de inspección y validación.**
- Se mencionó una estructura inicial del panel en una conversación anterior, pero no hay confirmación aquí de que el código actual exista, compile o esté conectado a datos reales.
- Inspeccionar archivos reales antes de reconstruir o duplicar trabajo.
- Objetivo de instalador: `Panel_Asistencias_Instalador.exe`.

### Bloque 6 — Prueba integral
**Estado: pendiente.** Debe probarse el ciclo completo usando las APK reales, con verificación del backend y Neon.

### Bloque 7 — Producción
**Estado: pendiente.** Despliegue estable del backend, configuración segura, pruebas con varios teléfonos, APK finales firmadas, instalador del Panel y documentación de entrega.

## 4. Ruta operativa, en orden

### Fase A — APK 0 del técnico (prioridad inmediata)

**A1. Identificar la versión correcta**
- Revisar el código fuente y los archivos disponibles.
- Identificar cuál versión produce el error y cuál fue la última versión conocida como funcional.
- No modificar archivos hasta ubicar el punto de fallo.

**A2. Aprovechar las pruebas existentes**
- Usar las pruebas de PowerShell como referencia: petición enviada, respuesta recibida y registro en Neon.
- No repetirlas si no existe una razón técnica concreta.

**A3. Diagnosticar el envío**
- Revisar la petición HTTP, URL de la API, formato del cuerpo, autenticación, respuesta HTTP, manejo de errores y estado de carga.
- Determinar por qué la interfaz permanece en «Enviando solicitud…» y por qué aparece «No se puede reenviar la solicitud de asistencia».
- Confirmar si la asistencia llegó a guardarse antes de intentar reenviar, para evitar duplicados.

**A4. Corregir con el cambio mínimo**
- Conservar la interfaz aprobada.
- No mezclar el ciclo de GPS con el envío de solicitudes.
- Si falla, conservar los datos del formulario y mostrar un error útil.
- Si el guardado se confirma, mostrar éxito y limpiar el formulario.

**A5. Compilar e instalar**
- Compilar la APK corregida y comprobar que se instala, abre y conserva la configuración de la API.
- Registrar qué versión/cambio se probó.

**A6. Prueba real desde el teléfono**
- Iniciar sesión.
- Crear una asistencia.
- Confirmar respuesta de la API y registro en Neon.
- Confirmar que desaparece el estado de carga y se muestra el resultado correcto.
- Verificar que no se crea una asistencia duplicada por reintentos.
- Confirmar que aparece en «Mis asistencias» con el mismo identificador.

**A7. Validar GPS del técnico por separado**
- Confirmar permisos, coordenadas y envío al backend.
- Mantener el intervalo aprobado de 5 segundos durante esta corrección.
- Verificar que el GPS no bloquea el botón de envío ni reinicia el formulario.

**Criterio de cierre de Fase A:** la APK real crea una asistencia, recibe y presenta el resultado correcto, no queda cargando, no duplica solicitudes y el GPS funciona de forma independiente.

### Fase B — APK del conductor

**B1. Login:** comprobar cédula, autenticación y perfil devuelto por el backend.  
**B2. Solicitudes disponibles:** confirmar consulta a la API y filtro por sede/permiso en el backend.  
**B3. Aceptación/asignación:** probar desde la APK y confirmar que la asignación queda registrada y no puede duplicarse.  
**B4. Estados:** validar las transiciones acordadas, incluido EN_CAMINO, llegada y EN_ASISTENCIA.  
**B5. GPS:** comprobar permisos, coordenadas y actualización real en backend.  
**B6. Finalización:** completar una asistencia desde la aplicación y verificar el estado guardado.

**Criterio de cierre de Fase B:** el conductor puede realizar el flujo desde la APK real, sin comandos manuales de PowerShell.

### Fase C — Prueba conjunta de las APK

1. El técnico inicia sesión y crea una asistencia.
2. La API registra la asistencia en Neon.
3. El conductor recibe la solicitud disponible.
4. El conductor acepta o se ejecuta la autoaceptación prevista.
5. Se confirma la asignación única.
6. El conductor actualiza el estado y la ubicación GPS.
7. La asistencia avanza hasta EN_ASISTENCIA.
8. El conductor completa el servicio.
9. Se comprueban historial, identificador y consistencia de datos.

Probar también errores de conexión y reintentos sin duplicar solicitudes.

**Criterio de cierre de Fase C:** el flujo completo funciona entre las APK reales y los registros corresponden en API y Neon.

### Fase D — Panel de control Windows

**D1. Inspeccionar el proyecto existente** y comprobar qué archivos y compilación están disponibles.  
**D2. Conectar el Panel con la API central**, nunca directamente con Neon.  
**D3. Implementar y validar los módulos aprobados:**
- Login con roles ADMIN/GESTOR.
- Dashboard y estados de asistencias.
- Mapa y seguimiento GPS.
- Solicitudes, asignación y reasignación.
- Conductores, técnicos, usuarios, sedes, roles y permisos.
- Vehículos, recursos, almuerzos y actividades.
- Estadísticas, reportes XLSX, importación Excel e historial de auditoría.
**D4. Probar las operaciones del gestor y administrador** y confirmar que los cambios se reflejan en backend y APK.  
**D5. Preparar y probar el instalador** `Panel_Asistencias_Instalador.exe`.

**Criterio de cierre de Fase D:** el Panel usa datos reales por API, los permisos funcionan y el instalador se prueba en Windows.

### Fase E — Integración definitiva y producción

**E1.** Desplegar el Backend/API en un servidor estable, sin depender del PC del usuario.  
**E2.** Verificar autenticación, permisos por rol/sede, validación de datos y protección de secretos.  
**E3.** Retirar mecanismos DEMO solo después de validar el circuito real.  
**E4.** Firmar y probar las APK finales y el instalador.  
**E5.** Probar con varios teléfonos y conductores, incluyendo solicitudes simultáneas.  
**E6.** Documentar instalación, configuración, operación y recuperación ante fallos.

**Criterio de cierre de Fase E:** sistema desplegado, seguro y probado en condiciones reales.

## 5. Próxima acción concreta

Empezar por **A1: inspeccionar el código fuente real de la APK 0 del técnico**. Localizar el ZIP o la versión más reciente disponible, comparar con la versión funcional y rastrear el error de envío antes de hacer cambios.

No comenzar por el Panel, no rehacer Neon y no repetir todo el backend. Primero cerrar el envío de solicitudes del técnico y comprobarlo desde el teléfono.

## 6. Registro de pruebas y cambios

Para cada cambio, registrar:
- Fecha y versión/archivo afectado.
- Problema observado.
- Cambio realizado.
- Prueba ejecutada y resultado real.
- Estado: pendiente, en curso, aprobado o bloqueado.
- Siguiente paso.

Solo actualizar a «aprobado» después de observar el resultado de la prueba. Si una prueba no se ha ejecutado, marcarla como pendiente; no inferir éxito.

## 7. Documentos relacionados

- `docs/ESTADO_CONTINUIDAD.md`
- `docs/PLAN_MAESTRO_PANEL_CONTROL.md`
- `docs/PLAN_MAESTRO_SISTEMA_ASISTENCIAS.md`
