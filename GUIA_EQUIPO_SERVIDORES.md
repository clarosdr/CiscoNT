# 📋 GUÍA RÁPIDA PARA EL EQUIPO: MÓDULO SERVIDORES/INFRAESTRUCTURA

## 🚀 ¿Qué es esto?
Se ha agregado un nuevo módulo completo para gestionar servidores e infraestructura sin afectar el sistema existente de órdenes de trabajo.

## ✅ Estado Actual
- ✅ **MIGRACIÓN COMPLETADA** - 11/11/2024 23:50
- ✅ **SIN PÉRDIDA DE DATOS** - Todas las tablas originales intactas
- ✅ **BACKUP DISPONIBLE** - Restauración posible si es necesario
- ✅ **CRUD FUNCIONAL** - API lista para usar

## 🔧 Cómo Usar el Nuevo Módulo

### 1. Endpoints de la API

```bash
# Listar todos los servidores
GET http://localhost:3000/servidores

# Obtener un servidor específico
GET http://localhost:3000/servidores/:id

# Crear nuevo servidor
POST http://localhost:3000/servidores
Content-Type: application/json

{
  "empresa": "Distribuidora XYZ",
  "nombre_servidor": "Servidor Bogotá 01",
  "vpn_nombre": "XYZ-BOGOTÁ",
  "vpn_contraseña": "123456",
  "vpn_ip": "10.10.10.22",
  "usuarios": [
    { "nombre": "Admin", "usuario": "diego", "contraseña": "pass", "rol": "admin" }
  ],
  "tailscale_tnet": "tecniserver.ts.net",
  "tailscale_config": { "key": "abc123", "autoapprove": true },
  "email_despliegue": "correo@empresa.com",
  "password_despliegue": "PASSWORD"
}

# Actualizar servidor (parcial)
PUT http://localhost:3000/servidores/:id
Content-Type: application/json

{
  "vpn_ip": "10.10.10.23",
  "vpn_contraseña": "newpassword"
}

# Eliminar servidor
DELETE http://localhost:3000/servidores/:id
```

### 2. Campos de la Tabla

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| `empresa` | string | ✅ Sí | Nombre de la empresa |
| `nombre_servidor` | string | ✅ Sí | Nombre identificativo |
| `vpn_nombre` | string | ❌ No | Nombre del perfil VPN |
| `vpn_contraseña` | string | ❌ No | Contraseña VPN |
| `vpn_ip` | string | ❌ No | IP en la VPN |
| `usuarios` | JSON | ❌ No | Array de usuarios del servidor |
| `tailscale_tnet` | string | ❌ No | Nombre del tailnet |
| `tailscale_config` | JSON | ❌ No | Config de Tailscale |
| `email_despliegue` | string | ❌ No | Email para Tailscale |
| `password_despliegue` | string | ❌ No | Password del email |

### 3. Ejemplos de Uso

#### Crear Servidor Mínimo
```json
{
  "empresa": "Mi Empresa",
  "nombre_servidor": "Server Principal"
}
```

#### Crear Servidor Completo
```json
{
  "empresa": "Tech Corp",
  "nombre_servidor": "Bogotá Data Center",
  "vpn_nombre": "TECH-BOG",
  "vpn_contraseña": "securepass123",
  "vpn_ip": "192.168.1.100",
  "usuarios": [
    {"nombre": "Admin Principal", "usuario": "admin", "contraseña": "hash123", "rol": "admin"},
    {"nombre": "Usuario Soporte", "usuario": "soporte", "contraseña": "hash456", "rol": "support"}
  ],
  "tailscale_tnet": "techcorp.ts.net",
  "tailscale_config": {
    "key": "tskey-123456789",
    "autoapprove": true,
    "tags": ["server", "production"]
  },
  "email_despliegue": "admin@techcorp.com",
  "password_despliegue": "emailpass123"
}
```

## 🔒 Seguridad

### Variables de Entorno Importantes
```bash
# En desarrollo (ya configuradas)
SERVIDORES_ENCRYPTION_KEY=your-secret-encryption-key-here
PASSWORD_HASH_ROUNDS=12
LOG_SERVIDORES_ENABLED=true

# En producción - CAMBIAR ESTOS VALORES
SERVIDORES_ENCRYPTION_KEY=<GENERAR_CLAVE_SEGURA>
```

### Mejores Prácticas
1. **Nunca** almacenar contraseñas en texto plano
2. **Siempre** usar HTTPS en producción
3. **Validar** IPs y formatos de entrada
4. **Limitar** acceso a endpoints según roles

## 🐛 Solución de Problemas

### Si la API no responde
```bash
# Verificar que el servidor esté corriendo
docker ps

# Ver logs del backend
docker logs gestorpro-backend-1

# Verificar conexión a base de datos
docker exec -it gestorpro_db psql -U admin -d gestorpro -c "SELECT * FROM servidores LIMIT 1;"
```

### Si hay error de conexión
```bash
# Verificar variables de entorno
cat backend/.env

# Reiniciar servicios
docker-compose restart
```

### Si necesito rollback
```bash
# Opción 1: Eliminar solo la tabla nueva
docker exec -i gestorpro_db psql -U admin -d gestorpro < db/migrations/03_rollback_servidores.sql

# Opción 2: Restaurar todo desde backup
docker exec -i gestorpro_db psql -U admin -d gestorpro < db/backups/gestorpro_backup_20251111_235009.sql
```

## 📊 Monitoreo

### Queries útiles para revisar datos
```sql
-- Total de servidores
SELECT COUNT(*) FROM servidores;

-- Servidores por empresa
SELECT empresa, COUNT(*) FROM servidores GROUP BY empresa;

-- Últimos servidores creados
SELECT * FROM servidores ORDER BY created_at DESC LIMIT 5;

-- Verificar integridad (todas las tablas)
SELECT 'ordenes_trabajo' as tabla, COUNT(*) FROM ordenes_trabajo
UNION ALL SELECT 'items_venta', COUNT(*) FROM items_venta
UNION ALL SELECT 'pagos', COUNT(*) FROM pagos
UNION ALL SELECT 'servidores', COUNT(*) FROM servidores;
```

## 🎯 Próximos Pasos

### Para Desarrolladores
1. **Probar** todos los endpoints con datos de prueba
2. **Validar** que los JSON de usuarios funcionen correctamente
3. **Implementar** validaciones adicionales si es necesario
4. **Agregar** autenticación y autorización

### Para QA
1. **Probar** CRUD completo con diferentes escenarios
2. **Validar** que no se afecten las órdenes de trabajo
3. **Verificar** manejo de errores
4. **Probar** rollback si es necesario

### Para DevOps
1. **Configurar** monitoreo de la nueva tabla
2. **Establecer** backups automáticos
3. **Configurar** alertas para errores
4. **Documentar** procedimientos de emergencia

## 📞 ¿Necesitas Ayuda?

### Antes de preguntar, verifica:
1. ✅ ¿El contenedor de DB está corriendo?
2. ✅ ¿El backend está respondiendo?
3. ✅ ¿Las variables de entorno están configuradas?
4. ✅ ¿El backup está disponible?

### Si todo falla:
1. 📋 Revisa el informe completo: `docs/MIGRATION_REPORT_SERVIDORES.md`
2. 🔧 Verifica configuración: `docs/CONFIG_SERVIDORES.md`
3. 🚨 Ejecuta rollback si es crítico
4. 📞 Contacta al equipo de desarrollo

---

**✅ EL MÓDULO ESTÁ LISTO PARA USAR - ¡ÉXITO EN TU DESARROLLO!**