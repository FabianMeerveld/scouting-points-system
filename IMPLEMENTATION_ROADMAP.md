# IMPLEMENTATION_ROADMAP.md

## Fase 0 - Basis setup (0.5 week)
- Repo structuur
- Django project init
- PostgreSQL connectie
- Env/config setup

## Fase 1 - Kern data & admin (1 week)
- Models + migraties voor kamp/team/deelnemer/activiteit/transactie/audit
- Django admin voor superadmin
- Kamp CRUD + team/deelnemer beheer

## Fase 2 - Leiding workflow (1 week)
- Kampcode login flow
- Puntentoekenning scherm (mobile-first)
- Ad-hoc activiteit toevoegen
- Input validatie puntenstappen

## Fase 3 - Leaderboard & dagoverzicht (0.5 week)
- Totale score aggregaties
- Dagtotalen + vandaag feed
- Display mode UI

## Fase 4 - Audit, correcties, export (0.5-1 week)
- Auditlog UI + filters
- Correctie via tegenboeking
- CSV export

## Fase 5 - Hardening & release (0.5 week)
- Security check (CSRF, cookies, rate limit)
- Performance tuning + indexcontrole
- UAT op telefoon + groot scherm

## Prioriteiten
- **P0:** login, transacties, leaderboard totaal/dag
- **P1:** auditlog, ad-hoc activiteiten, admin beheer
- **P2:** export, extra statistieken, UX polish

## Effort schatting
- MVP: 3–4 weken parttime
- Productie-klaar: 4–6 weken inclusief test/iteratie
