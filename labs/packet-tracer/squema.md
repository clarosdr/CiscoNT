## 🧩 Diagrama ERD (texto plano)

Representación simplificada del esquema de la tabla `servidores`, sin relaciones externas:

+----------------------+ | servidores | +----------------------+ | id UUID (PK) | | empresa TEXT | | nombre_servidor TEXT | | vpn_nombre TEXT | | vpn_contraseña TEXT | | vpn_ip TEXT | | usuarios JSONB | | tailscale_tnet TEXT | | tailscale_config JSONB | | email_despliegue TEXT | | password_despliegue TEXT | | created_at TIMESTAMP | +----------------------+

Código

🟡 Esta tabla es **independiente** y no se relaciona con otras del sistema.  
🧠 Campos `usuarios` y `tailscale_config` permiten flexibilidad mediante estructuras JSONB.  
🔐 Clave primaria: `id` (UUID autogenerado).  
📅 Auditoría: `created_at` con timestamp automático.
