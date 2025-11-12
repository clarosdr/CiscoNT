# 📌 Módulo Servidor / Infraestructura – GestorPro

## 🎯 Objetivo
Agregar un nuevo módulo administrativo al backend existente llamado **Servidor / Infraestructura**, sin afectar el módulo de **Órdenes de Trabajo (OT)**.  
Este módulo es **independiente** y se centra en la gestión de servidores corporativos, sus accesos y configuraciones.

---

## 🗄️ 1. Base de Datos (PostgreSQL)

Archivo: `db/init/01_schema.sql`

### Tabla: `servidores`

```sql
CREATE TABLE IF NOT EXISTS servidores (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

    empresa TEXT NOT NULL,                     
    nombre_servidor TEXT NOT NULL,             

    vpn_nombre TEXT,                           
    vpn_contraseña TEXT,                       
    vpn_ip TEXT,                               

    usuarios JSONB,                            
    tailscale_tnet TEXT,                       
    tailscale_config JSONB,                    

    email_despliegue TEXT,                     
    password_despliegue TEXT,                  

    created_at TIMESTAMP DEFAULT NOW()
);
📐 Esquema y Relaciones
Tabla independiente: No se relaciona con ordenes_trabajo ni otras tablas existentes.

Campo usuarios (JSONB): Permite almacenar múltiples usuarios asociados al servidor.

Campo tailscale_config (JSONB): Flexibilidad para guardar configuraciones dinámicas (keys, nodos, flags).

Clave primaria: id (UUID autogenerado).

Timestamps: created_at para auditoría.

⚙️ 2. Backend Express (CRUD)
Archivo: backend/src/routes/servidores.js

Endpoints disponibles
GET / → Obtener todos los servidores

GET /:id → Obtener un servidor por ID

POST / → Crear nuevo servidor

PUT /:id → Actualizar servidor existente

DELETE /:id → Eliminar servidor

🌐 3. Registro de Ruta en Express
Archivo: backend/src/server.js

js
import servidoresRoutes from "./routes/servidores.js";
app.use("/servidores", servidoresRoutes);
🧪 4. Endpoints de Prueba
GET → http://localhost:3000/servidores

POST → http://localhost:3000/servidores

PUT → http://localhost:3000/servidores/:id

DELETE → http://localhost:3000/servidores/:id

Ejemplo de creación (POST)
json
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
🚫 Restricciones
No modificar ni tocar el módulo de Órdenes de Trabajo (OT).

Este módulo es totalmente independiente.

📊 Diagrama de Esquema (simplificado)
Código
+------------------+
|    servidores    |
+------------------+
| id (UUID)        |
| empresa          |
| nombre_servidor  |
| vpn_nombre       |
| vpn_contraseña   |
| vpn_ip           |
| usuarios (JSONB) |
| tailscale_tnet   |
| tailscale_config |
| email_despliegue |
| password_despliegue |
| created_at       |
+------------------+
