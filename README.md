# Scouting Zomerkamp Puntenteller

Planning/design repository voor een puntenteller web-app voor scoutingkampen.

## Inhoud
- `PRODUCT_SPEC.md` — user stories (30+) en acceptance criteria
- `TECHNICAL_ARCHITECTURE.md` — stack, componenten en ontwerpkeuzes
- `DATABASE_SCHEMA.md` — ER-model en SQL schema
- `API_SPEC.md` — REST endpoints met voorbeelden
- `UI_WIREFRAMES.md` — wireframes voor admin/leiding/leaderboard
- `IMPLEMENTATION_ROADMAP.md` — fases, prioriteiten en effort
- `DEPLOYMENT_GUIDE.md` — Raspberry Pi deployment met PostgreSQL en Django
- `PROJECT_STRUCTURE.md` — folder layout en initiële bestanden

## Quick start (implementatiefase)
1. Maak virtual environment en activeer.
2. Installeer dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Kopieer `.env.example` naar `.env` en vul waarden in.
4. Start Django-projectimplementatie volgens roadmap.

## Doel
Een mobielvriendelijke web-app met kampcode-login, punten in stappen van 5, dagtotalen/totaalscore leaderboard en volledige audit logs.
