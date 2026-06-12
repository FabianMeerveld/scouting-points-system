# TECHNICAL_ARCHITECTURE.md

## Stack
- **Backend:** Django 5 + Django REST Framework
- **Database:** PostgreSQL 16
- **Frontend:** Django Templates + HTMX/Alpine.js (lichtgewicht, mobiel, snel op Raspberry Pi)
- **Auth:** Django admin auth + kampcode-sessie-auth
- **Deploy:** Gunicorn + Nginx + systemd op Raspberry Pi OS

## Architectuuroverzicht
1. Browser (leiding/admin/display) verstuurt HTTP requests.
2. Django app verwerkt auth, validatie, business rules.
3. DRF endpoints leveren JSON voor leaderboard/puntenmutaties.
4. PostgreSQL bewaart kampen, teams, activiteiten, transacties en auditlogs.

## Componenten
- `apps/camps`: kamp lifecycle, inlogcodes
- `apps/teams`: teams en deelnemers
- `apps/activities`: standaard + ad-hoc activiteiten
- `apps/scoring`: puntentransacties, correcties, aggregates
- `apps/audit`: append-only audit events
- `apps/dashboard`: leaderboard queries en presentatie

## Belangrijke ontwerpkeuzes
- **Append-only scoring**: geen hard delete op transacties; correcties via tegenboeking.
- **Kampscheiding**: alle querysets scoped op `camp_id`.
- **Puntvalidatie**: whitelist op stappen van 5 met grenzen (-15..50, exclusief 0).
- **Simpel realtime model**: update bij refresh (geen websockets vereist in MVP).

## API Design Principles
- RESTful resources onder `/api/v1/`.
- JSON responses met consistente envelope: `data`, `meta`, `errors`.
- Server-side pagination voor logs en transactielijsten.
- Optimistische locking via `updated_at` voor admin-mutaties (optioneel fase 2).

## Security
- CSRF-protectie voor form-based acties.
- Secure cookies, HttpOnly, SameSite=Lax.
- Rate limiting op logincode endpoint.
- Audit op alle create/update/delete acties.

## Performance
- Indexen op `camp_id`, `team_id`, `timestamp`.
- Materialized view (optioneel) voor zware historiek-rapportages.
- Aggregaties via SQL `SUM` en date filters.
