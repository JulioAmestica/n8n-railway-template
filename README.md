
# n8n on Railway (with Supabase Postgres)

Deploy a **zero-cost** n8n stack using **Railway** for compute and **Supabase** (Free) for PostgreSQL.

## 1‑Click Deploy

[![Deploy on Railway](https://railway.app/button.svg)](
https://railway.app/new?template=https%3A%2F%2Fgithub.com%2FJulioAmestica%2Fn8n-railway-template
)

> **Cómo usar el botón:**  
> 1) Sube este repo a tu GitHub (p.ej. `julioamestica/n8n-railway-template`).  
> 2) Reemplaza `<REPLACE_WITH_YOUR_REPO_URL>` en el link de arriba por la URL pública de tu repo.  
> 3) Haz clic en el botón y sigue el asistente de Railway.

---

## Variables de entorno requeridas

Crea las variables en Railway (o usa el *Deploy Template* que te las pedirá). Copia desde `.env.example`:

```
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=aws-1-us-east-1.pooler.supabase.com
DB_POSTGRESDB_PORT=6543
DB_POSTGRESDB_DATABASE=postgres
DB_POSTGRESDB_USER=<TU_USUARIO_SUPABASE>
DB_POSTGRESDB_PASSWORD=<TU_PASSWORD_SUPABASE>

# Seguridad / Región
N8N_ENCRYPTION_KEY=<CLAVE_GENERADA_CON_OPENSSL>
GENERIC_TIMEZONE=America/Santiago

# Host/URL (ajústalas después del primer deploy con tu dominio Railway)
N8N_PROTOCOL=https
N8N_HOST=
WEBHOOK_URL=

# Opcionales recomendados
N8N_DIAGNOSTICS_ENABLED=false
N8N_VERSION_NOTIFICATIONS_ENABLED=false
```
Genera una clave segura para `N8N_ENCRYPTION_KEY`:
```bash
openssl rand -hex 32
```

### Dónde obtengo los datos de Supabase
En `Project Settings → Database`: **HOST**, **DB**, **USER**, **PASSWORD**.  
Recomendado: usar el **pooler** (puerto 6543).

---

## Dockerfile

Railway construirá la imagen desde este `Dockerfile` minimal que apunta a la imagen oficial de n8n:

```Dockerfile
FROM n8nio/n8n:latest
```

No necesitas más. Railway detecta el `PORT` automáticamente; n8n expone `5678`.

---

## HTTPS y dominio

Railway dará una URL pública con HTTPS. Después del primer deploy, ve a **Settings → Domains** y copia el dominio asignado:
- Ajusta `N8N_HOST` con **ese dominio** (sin protocolo)
- Ajusta `WEBHOOK_URL` con `https://<tu-dominio>`

Reinicia el servicio para que tome los cambios.

---

## Persistencia

Guardamos **workflows y credenciales en Postgres** (Supabase). Con `N8N_ENCRYPTION_KEY` las credenciales quedan cifradas. **No necesitas volúmenes**.

---

## Desarrollo local (opcional)

Puedes probar localmente con Docker y `docker-compose.yml` incluido:
```bash
cp .env.example .env
# rellena .env con tu Supabase y ENCRYPTION_KEY
docker compose up -d
```

Luego abre http://localhost:5678

---

## Seguridad

- Usa una `N8N_ENCRYPTION_KEY` robusta.  
- Mantén privados los secretos.  
- Activa 2FA en tus cuentas de Railway y Supabase.

---

## Licencia

MIT
