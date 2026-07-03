# CLAUDE.md — [NOMBRE DEL PRODUCTO]

> PLANTILLA: sustituye [NOMBRE DEL PRODUCTO], rellena la sección "Contexto",
> y borra este bloque. Las reglas de más abajo son permanentes del track A: no las borres.

## Contexto

- **Producto:** [NOMBRE DEL PRODUCTO] — white-label, se revende a múltiples clientes
- **Track:** A — producto revendible, una instancia (Vercel + Supabase) por cliente
  (estándar: `~/.claude/portfolio/PORTFOLIO_STANDARDS.md`)
- **Spec de referencia:** `SDD_MASTER.md` en la raíz de este repo. Si código y SDD se
  contradicen, gana el SDD — o se actualiza el SDD en la misma sesión (protocolo de
  sincronización de specs del CLAUDE.md global).

## Stack (no cambiar sin registrarlo en DECISIONS.md)

- Next.js (App Router) + TypeScript + Tailwind
- Supabase: Postgres + Auth + Storage (un proyecto NUEVO por cliente)
- Despliegue: Vercel (un proyecto por cliente)
- Patrones de código y SQL: skill `sdd-transformer` → `stack.md` y `sql-patterns.md`

## Reglas permanentes de este proyecto (track A)

- **Toda tabla tiene RLS habilitado** con sus policies — sin excepciones, incluidas
  tablas "internas". Una tabla sin RLS es un bug de seguridad, no una optimización.
- **Nunca `organization_id` ni multi-tenancy**: una instancia por cliente por diseño.
  Si un día el modelo cambia, es una decisión de portfolio, no de este repo.
- Toda mutación registra en `activities` (append-only, nunca UPDATE/DELETE sobre esa tabla).
- El cliente admin de Supabase (`service_role`) solo se usa en Route Handlers del servidor,
  nunca en componentes ni en código que llegue al navegador.
- Todo cron (Vercel Cron / scheduled functions) pasa por el patrón cron-lock: protección
  contra ejecuciones solapadas + registro de ejecución con status/started_at/finished_at.
- Los endpoints de cron se protegen con `CRON_SECRET` (header), nunca abiertos.
- Toda entidad con `status` respeta su FSM del SDD; no inventar transiciones no documentadas.
- Cambios de esquema SOLO vía migraciones en `supabase/migrations/` — nunca a mano desde
  el dashboard de Supabase (el dashboard vale para consultar, no para alterar esquema).
- Secretos solo en `.env.local` (excluido por .gitignore) y en las env vars de Vercel.

## Comandos

```bash
# desarrollo
npm run dev

# lo mismo que ejecuta la CI
npm run lint
npm run build

# migraciones (Supabase CLI)
supabase migration new descripcion_del_cambio
supabase db push          # aplicar a la instancia enlazada
```
