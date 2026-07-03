# Template — Track A (Producto revendible, white-label)

Plantilla de arranque para proyectos **Track A** del portfolio: producto que se revende
tal cual a muchos clientes, una instancia por cliente, con Next.js + Supabase + TypeScript.

> Referencia del estándar: `~/.claude/portfolio/PORTFOLIO_STANDARDS.md`
> Referencia técnica del stack: skill `sdd-transformer` → `stack.md`

---

## Cómo estrenar un proyecto con esta plantilla

1. En GitHub: botón **"Use this template"** → nombra el nuevo repo → clónalo.
2. Ejecuta la skill `sdd-transformer` con el brief del proyecto → guarda el resultado
   como `SDD_MASTER.md` en la raíz de este repo.
3. Revisa los `[ASSUMED]` del SDD y cierra el checklist de su Sección 15.
4. **Scaffolding de la app** (a diferencia del track B, aquí la app ES el repo entero):
   ```bash
   npx create-next-app@latest . --typescript --tailwind --eslint --app --no-src-dir
   ```
   Ejecutado en la raíz — create-next-app respeta los archivos existentes de la plantilla.
   Si protesta por conflicto, scaffoldea en una carpeta temporal y mueve el contenido aquí.
5. Instala el cliente de Supabase: `npm install @supabase/supabase-js @supabase/ssr`
6. Crea el proyecto en Supabase (uno NUEVO por cliente — nunca compartido) y copia
   `.env.example` a `.env.local` con sus claves.
7. Sustituye este README por el del proyecto real.
8. Borra las secciones "PLANTILLA:" de `CLAUDE.md`, `PROGRESS.md` y `DECISIONS.md`.
9. Primer commit: `chore: bootstrap desde template-track-a` + push. La CI se activará
   sola en cuanto exista `package.json`.

## Qué incluye ya

- `CLAUDE.md` local base (reglas track A: RLS, cron-lock, activities, sin organization_id)
- `PROGRESS.md` / `DECISIONS.md` con la estructura vacía
- `.gitignore` con secretos y artefactos de Next excluidos desde el minuto uno
- `.env.example` con las variables estándar del stack (Supabase, CRON_SECRET...)
- `supabase/migrations/` con las convenciones de migración
- CI de GitHub Actions: lint + build (se salta con aviso hasta que exista `package.json`)

## Qué NO incluye (a propósito — complejidad justa)

- La app Next.js generada → se crea con `create-next-app` en el paso 4 (congelar el
  scaffolding en la plantilla envejece mal; el generador oficial siempre da la versión buena).
- Modelos, endpoints o pantallas de negocio → los define el `SDD_MASTER.md`.
- Despliegue automático a Vercel, observabilidad externa → no pertenecen a Fase 1.

## Modelo de instancias (recordatorio clave del track A)

**Una instancia completa por cliente:** un proyecto de Supabase + un proyecto de Vercel
por cada cliente que compra el producto. Nunca `organization_id`, nunca datos de dos
clientes en la misma base de datos. El "despliegue de un cliente nuevo" es: duplicar
proyecto Vercel + crear proyecto Supabase + aplicar migraciones + configurar env vars.
