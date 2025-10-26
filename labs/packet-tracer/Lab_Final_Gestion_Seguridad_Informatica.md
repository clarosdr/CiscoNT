# Laboratorio Avanzado de Gestión de Seguridad Informática

**Universidad Cooperativa de Colombia**  
**Facultad de Ingeniería – Ingeniería de Sistemas**  
**Curso: Gestión de Seguridad Informática**

---

## 🎯 Objetivo del Laboratorio

El objetivo de este laboratorio es que el estudiante configure y asegure una infraestructura de red corporativa, aplicando políticas de seguridad que limiten el acceso no autorizado.

Se trabajará sobre una topología en **GNS3** con dispositivos **Cisco** y un **servidor** que cumplirá múltiples funciones.

El enfoque se centrará primero en la configuración segura de los **servidores** y sus políticas, y posteriormente en la configuración de **red**.

---

## ✅ 1. Configuración de Servidores y Políticas de Seguridad

Proteger los recursos críticos de la organización, asegurando acceso controlado por identidad, horarios y privilegios.

---

### 1.1 Controlador de Dominio

Centraliza la gestión de usuarios, grupos y políticas de seguridad.

#### Tareas:
- Instalar Windows Server (2019/2012 R2) o equivalente (Samba4).
- Configurar Active Directory y unir estaciones al dominio.
- Crear usuarios: **Raquel, Andrea, Iván y Paula**.
- Políticas iniciales:
  - Cambio forzado de contraseña en primer inicio.
  - Contraseña mínima de **10 caracteres** incluyendo letras, números y símbolos especiales.
  - Bloqueo tras **2 intentos fallidos**.
- Permisos y horarios:

| Usuario | Horario | Permisos |
|--------|---------|----------|
| Iván | L-M-V 6:00 p.m. – 10:00 p.m. / Sáb 8:00 a.m. – 12:00 m. | Apagar el sistema. Eliminar cuentas |
| Raquel | Todos los días 2:00 p.m. – 10:00 p.m. | Apagar sistema |
| Andrea | Todos los días 2:00 p.m. – 10:00 p.m. | Apagar sistema. Eliminar cuentas |
| Paula | L-V 8:00 a.m. – 1:00 p.m. | Solo en PC asignada |

#### Seguridad adicional:
- Auditoría de inicios y cambios de cuentas.
- Caducidad máxima de contraseña: **90 días**.
- No reusar últimas **5 contraseñas**.
- Bloqueo de cuenta **15 min** por intentos fallidos.
- Activar MFA en cuentas privilegiadas.
- Habilitar LDAP Signing.
- GPO: deshabilitar cuentas inactivas > 30 días.

---

### 1.2 Servidor de Archivos

Directorio: **RAI&CA**  
Archivos: **EXA1** y **EXA2**

| Usuario | EXA1 | EXA2 |
|--------|------|------|
| Iván | Solo lectura | - |
| Andrea | Control total | Control total |
| Raquel | Lectura/Escritura | Solo lectura |
| Paula | Solo lectura | Sin acceso |

---

### 1.3 Políticas del Sistema

| Usuario | Restricción |
|--------|------------|
| Raquel | Sin Panel de Control |
| Iván | No cambiar fondo de pantalla |
| Andrea | Única con acceso a configuración de pantalla |
| Paula | Sin acceso a puertos USB |

Medidas extra:
- Desactivar ejecución automática de medios extraíbles.
- Bloqueo por inactividad: **10 min**.
- Restricción de herramientas administrativas.
- Monitoreo de acceso a recursos compartidos.
- Implementar AppLocker o equivalente.

---

### 1.4 Configuración AAA con RADIUS

Autenticación centralizada integrada con Active Directory.

#### En el servidor:
- Registrar router Cisco como **cliente RADIUS** (IP + clave).
- Crear usuarios con mismas políticas del AD.
- Habilitar logs de autenticación.

Sistemas soportados:
- Windows Server → **NPS**
- Linux → **FreeRADIUS** + **LDAP/Samba4**

---

## 🔐 2. Configuración de Red y Seguridad Cisco

Aplica segmentación, control de acceso y mitigaciones de capa 2 y 3.

---

### 2.1 VLANs y Enrutamiento Inter-VLAN

- VLAN 10 → **Estudiantes**
- VLAN 20 → **Profesores**
- Router-on-a-Stick para interconexión controlada

---

### 2.2 Seguridad en Puertos

Evita accesos y ataques por capa 2.

#### 2.2.1 Port Security
- Activar Port Security en puertos de acceso.
- Límite de MACs por puerto.
- Definir acción ante violación.
- Verificar estado y direcciones aprendidas.

#### 2.2.2 DHCP Snooping
- Activar en VLANs especificadas.
- Definir puertos de confianza (uplinks a servidor/router).
- Limitar paquetes DHCP por segundo en puertos no confiables.
- Validar que solo el servidor DHCP asigne IP.

#### Recomendaciones adicionales:
- BPDU Guard / Root Guard según topología.
- Control de tormentas broadcast/multicast.
- Documentar resultados y eventos detectados.

---
