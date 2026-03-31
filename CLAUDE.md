# CLAUDE.md — Vertical Inmobiliaria

---

## Arquitectura (respetar siempre)

🔴 **CORE — nunca modificar:** `/lib/supabase`, `/middleware.ts`, `/components/ui`, `/types/index.ts`
🟡 **CONFIG — solo por cliente:** `/config/site.ts`, `/config/features.ts`
🟢 **EXTENSIÓN — agregar libremente:** `/app/[feature]/`, `/actions/`, `/validations/`, `/components/[vertical]/`

**Prohibiciones clave:** no usar `any`, no crear Server Actions sin ZSA, no tocar CORE, no hardcodear nombre del cliente (usar `siteConfig.name`), no agregar dependencias sin preguntar.

---

## Contexto del dominio

Sistema de gestión de alquileres para inmobiliarias argentinas. Maneja contratos residenciales, comerciales y de galpón con ajuste por índice.

### Entidades

| Entidad | Descripción |
|---------|-------------|
| `Contrato` | Contrato de alquiler. Vincula Inmueble + Locatario + condiciones |
| `Inmueble` | Propiedad alquilada (domicilio, datos catastrales, tipo) |
| `Persona` | Locador o locatario (datos personales, CUIL) |
| `Garante` | Garante del contrato (datos personales, bien en garantía) |
| `Condiciones` | Monto, duración, índice de ajuste, periodicidad |

### Tipos de contrato
- `vivienda` — residencial
- `comercial` — local comercial
- `galpon` — depósito / galpón industrial

### Reglas de negocio
- Ajuste de alquiler por **IPC o ICL** (a elección), con periodicidad configurable: mensual, trimestral, cuatrimestral, semestral
- Marco legal: **DNU 70/2023** — libertad contractual, sin plazos mínimos forzados
- RLS en Supabase: cada usuario solo ve sus propios contratos

---

## Server Actions ya implementadas

| Acción | Descripción |
|--------|-------------|
| `listarContratoAction()` | Lista contratos del usuario autenticado |
| `crearContratoAction(data)` | Crea nuevo contrato |
| `actualizarContratoAction(id, data)` | Actualiza / autosave |
| `duplicarContratoAction(id)` | Duplica contrato con "(copia)" |
| `eliminarContratoAction(id)` | Elimina contrato |

---

## Módulos disponibles

```
hasContracts          → gestión de contratos de alquiler
hasProperties         → listado y gestión de propiedades
hasTenants            → gestión de inquilinos y contactos
hasBilling            → facturación
hasReports            → reportes y estadísticas
hasEmailNotifications → notificaciones por email
```
