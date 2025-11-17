# Informe de Laboratorio Final – Gestión de Seguridad Informática

## 🎯 Objetivo
Configurar y asegurar una infraestructura de red corporativa en VirtualBox con Windows Server 2012 R2 y dispositivos Cisco en GNS3, aplicando políticas de seguridad que limiten el acceso no autorizado.

---

## 1. Instalación del Servidor
**Descripción:** Instalación de Windows Server 2012 R2 en VirtualBox y configuración inicial.

- Captura 1: Pantalla de instalación del sistema operativo.
- Captura 2: Configuración de red en VirtualBox.
- Comentario: Se eligió adaptador puente para permitir comunicación con la red simulada.

---

## 2. Configuración del Controlador de Dominio
**Descripción:** Instalación de Active Directory y creación de usuarios.

- Captura 3: Instalación del rol AD DS.
- Captura 4: Promoción a controlador de dominio (`corp.local`).
- Captura 5: Usuarios creados (Raquel, Andrea, Iván, Paula).
- Comentario: Se configuró política de contraseña mínima de 10 caracteres, caducidad de 90 días y bloqueo tras 2 intentos fallidos.

---

## 3. Políticas de Seguridad
**Descripción:** Restricciones de acceso y auditoría.

- Captura 6: Configuración de horarios de inicio de sesión por usuario.
- Captura 7: GPO con políticas de bloqueo y auditoría.
- Captura 8: Event Viewer mostrando logs de inicio/cierre de sesión.
- Comentario: Se habilitó LDAP Signing y MFA para cuentas privilegiadas.

---

## 4. Servidor de Archivos
**Descripción:** Creación de carpeta `RAI&CA` y asignación de permisos NTFS.

- Captura 9: Estructura de carpetas y archivos (EXA1, EXA2).
- Captura 10: Propiedades de seguridad mostrando permisos de cada usuario.
- Captura 11: Prueba de acceso con cada cuenta.
- Comentario: Andrea tiene control total, Iván solo lectura, Raquel lectura/escritura en EXA1 y Paula solo lectura en EXA1.

---

## 5. Políticas del Sistema
**Descripción:** Restricciones adicionales mediante GPO.

- Captura 12: Bloqueo de Panel de Control para Raquel.
- Captura 13: Restricción de cambio de fondo para Iván.
- Captura 14: Denegación de acceso a USB para Paula.
- Comentario: Se configuró bloqueo automático tras 10 minutos de inactividad y AppLocker para listas blancas de aplicaciones.

---

## 6. Configuración de AAA con RADIUS (NPS)
**Descripción:** Integración de NPS con Active Directory y router Cisco.

- Captura 15: Instalación del rol NPAS.
- Captura 16: Registro del router como cliente RADIUS.
- Captura 17: Políticas de acceso con horarios y privilegios.
- Comentario: Se habilitaron logs de autenticación y monitoreo de intentos fallidos.

---

## 7. Configuración de Red en Cisco (GNS3)
**Descripción:** Segmentación por VLAN y seguridad de puertos.

- Captura 18: Configuración de VLANs 10 (Estudiantes) y 20 (Profesores).
- Captura 19: Router-on-a-Stick con subinterfaces.
- Captura 20: Port Security y DHCP Snooping configurados.
- Comentario: Se añadieron medidas complementarias como BPDU Guard y Storm Control.

---

## ✅ Conclusiones
- Se implementaron políticas de seguridad en servidores y red para proteger recursos críticos.
- Se validó el acceso controlado por identidad, horario y privilegios.
- Se documentaron pruebas de acceso y restricciones aplicadas con pantallazos.
- La infraestructura cumple con los objetivos del laboratorio universitario.
