# Estado de continuidad — 8 de octubre de 2026

## Objetivo
Sistema de asistencias técnicas compuesto por APK Técnico, Panel de Control Windows y APK Conductor. Todos usan el Backend/API y Neon PostgreSQL; los clientes no se conectan directamente a la base de datos.

## Reglas confirmadas
- El técnico crea una solicitud de escalera/recurso con dirección y observación.
- Solo tras confirmar el guardado, mostrar «Solicitud enviada correctamente» y limpiar el formulario.
- Si falla, conservar los datos y mostrar el error real.
- GPS y envío de solicitudes deben ser independientes. El GPS no debe bloquear el botón ni reiniciar el formulario.
- Mantener el GPS en 5 segundos durante la corrección inmediata; no mostrar conteo repetitivo en el menú.
- La solicitud aparece en «Mis asistencias» desde que se crea, inicialmente sin conductor asignado.
- El gestor puede asignar/reasignar desde el Panel; un conductor también puede autoaceptar una solicitud disponible de su sede.
- La autoaceptación debe impedir doble asignación y la misma solicitud conserva su identificador durante todo el ciclo.
- Conservar la interfaz aprobada del técnico; no rehacerla ni tocar conductor/backend sin evidencia.

## Estado actual
- Repositorio: alexracenga-creator/apk-para-asistencia.
- Ya existe documentación del Plan Maestro en docs/PLAN_MAESTRO_PANEL_CONTROL.md y docs/PLAN_MAESTRO_SISTEMA_ASISTENCIAS.md.
- APK Conductor: el login pasó una prueba anterior; falta prueba integral.
- APK Técnico: después de una modificación reciente, el envío queda en «Enviando solicitud…», no limpia el formulario y aparece «No se puede reenviar la solicitud de asistencia». Causa aún no confirmada.
- Panel de Control: todavía por construir; existe una referencia visual futurista azul/cian guardada en el proyecto.
- Backend: anteriormente se iniciaba con npm.cmd start desde la carpeta backend_final_work y escuchaba en puerto 3000. Las IP locales citadas antes fueron 192.168.1.234 y 192.168.1.215; verificar la IP actual antes de usarla.

## Siguiente paso
1. Recuperar y comparar el código actual de la APK técnica con la última versión que enviaba correctamente.
2. Inspeccionar la función de envío y la respuesta HTTP real; no adivinar la causa.
3. Corregir solo el bloqueo de envío, separar el GPS y mostrar errores concretos.
4. Compilar y probar contra el backend antes de declarar el arreglo terminado.
5. Después validar el flujo de conductor y construir el Panel conforme al Plan Maestro.

## Criterio final
Técnico → Backend → Panel/gestor o autoaceptación del conductor → GPS → llegada → finalización → historial y estadísticas.

No guardar credenciales ni secretos en este repositorio.