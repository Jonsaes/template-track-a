# Migraciones (Supabase)

Toda migración nace del SDD (Sección 8, SQL) y vive aquí como archivo versionado.

```bash
supabase migration new descripcion_del_cambio   # crea el archivo
supabase db push                                 # aplica a la instancia enlazada
```

Reglas:
- Nunca alterar el esquema desde el dashboard de Supabase — solo vía estos archivos.
- Toda tabla nueva incluye `ALTER TABLE ... ENABLE ROW LEVEL SECURITY` + sus policies
  en la MISMA migración (patrones en sql-patterns.md de la skill sdd-transformer).
- Para desplegar un cliente nuevo: crear proyecto Supabase → aplicar TODAS las
  migraciones en orden → configurar env vars en Vercel.
