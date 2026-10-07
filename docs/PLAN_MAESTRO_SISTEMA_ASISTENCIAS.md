# Plan Maestro — Sistema de Asistencias

Documento de continuidad del proyecto.

## Objetivo
Construir un sistema centralizado de asistencias técnicas compuesto por APK para Técnicos, APK para Conductores y Panel de Control, compartiendo Backend y Neon PostgreSQL.

## Piezas
- APK Técnicos: iniciar sesión, solicitar asistencia/recurso, ubicación, observaciones, consultar estado, conductor asignado, seguimiento y cierre.
- APK Conductores: iniciar sesión, recibir asistencias, aceptar, EN_CAMINO, GPS, llegada, EN_ASISTENCIA, completar y quedar disponible.
- Panel de Control: administrar usuarios, solicitudes, asignaciones, mapa, supervisión y estadísticas.

## Flujo
Técnico solicita → Backend registra → Gestor visualiza → asigna conductor → conductor recibe/acepta → recorrido/GPS → llegada → asistencia → cierre → estadísticas.

## GPS y estadísticas
El conductor enviará posición aproximadamente cada 4 segundos. El Backend almacena y distribuye la información. Se conservarán posiciones periódicas para calcular distancia acumulada, tiempos y estadísticas.

## Fases
1. Base de datos Neon.
2. Backend/API.
3. Panel de Control.
4. Conectar APK Técnico al backend real.
5. Conectar APK Conductor al backend real.
6. Prueba integral real: solicitud → panel → asignación → conductor → GPS → llegada → cierre → estadísticas.
7. Retirar definitivamente mecanismos y datos DEMO después de validar el circuito real.

## Principio de estabilidad
Las últimas APK compiladas quedan como versiones base. No se debe rehacer su interfaz sin necesidad; primero se construye la infraestructura real y después se conectan al backend.

## Estado de continuidad
Neon y Backend están funcionando en el entorno actual. El Panel de Control todavía está por construir. Las APK están en proceso de conexión/prueba contra el backend real.

> Este documento no contiene credenciales ni secretos.
