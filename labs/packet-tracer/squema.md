## 🗄️ Esquema técnico – Tabla `servidores`

Tabla independiente del módulo "Servidor / Infraestructura", sin relaciones externas.

### 📋 Estructura de campos

| Campo                 | Tipo de dato     | Descripción                                         |
|----------------------|------------------|-----------------------------------------------------|
| `id`                 | `UUID` (PK)      | Identificador único del servidor                   |
| `empresa`            | `TEXT`           | Nombre de la empresa asociada                      |
| `nombre_servidor`    | `TEXT`           | Nombre del servidor (ej: "Servidor Bogotá 01")     |
| `vpn_nombre`         | `TEXT`           | Nombre del perfil VPN (Radmin)                     |
| `vpn_contraseña`     | `TEXT`           | Contraseña del perfil VPN                          |
| `vpn_ip`             | `TEXT`           | IP asignada en la VPN                              |
| `usuarios`           | `JSONB`          | Lista de usuarios con credenciales y roles         |
| `tailscale_tnet`     | `TEXT`           | Nombre del tailnet (ej: tecniserver.ts.net)        |
| `tailscale_config`   | `JSONB`          | Configuración adicional de Tailscale               |
| `email_despliegue`   | `TEXT`           | Correo usado para el despliegue                    |
| `password_despliegue`| `TEXT`           | Contraseña del correo de despliegue                |
| `created_at`         | `TIMESTAMP`      | Fecha de creación del registro                     |

---

### 🔗 Relaciones

| Tabla        | Relación | Tipo | Comentario                         |
|--------------|----------|------|------------------------------------|
| `servidores` | —        | —    | Tabla independiente, sin relaciones |

---

### 🧠 Notas técnicas

- `usuarios` y `tailscale_config` usan `JSONB` para permitir estructuras flexibles.
- No hay claves foráneas ni dependencias externas.
- El campo `created_at` se autogenera con `NOW()`.

---

### 📌 Ejemplo de contenido en `usuarios`

```json
[
  {
    "nombre": "Admin",
    "usuario": "diego",
    "contraseña": "pass",
    "rol": "admin"
  }
]
✅ Estado del módulo
[x] Tabla creada en PostgreSQL

[x] CRUD completo en Express

[x] Documentación técnica lista
