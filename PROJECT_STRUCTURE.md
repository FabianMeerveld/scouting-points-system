# PROJECT_STRUCTURE.md

## Voorgestelde mapstructuur
```text
scouting-points-system/
├── README.md
├── requirements.txt
├── .env.example
├── .gitignore
├── PRODUCT_SPEC.md
├── TECHNICAL_ARCHITECTURE.md
├── DATABASE_SCHEMA.md
├── API_SPEC.md
├── UI_WIREFRAMES.md
├── IMPLEMENTATION_ROADMAP.md
├── DEPLOYMENT_GUIDE.md
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── apps/
│   ├── camps/
│   ├── teams/
│   ├── activities/
│   ├── scoring/
│   ├── dashboard/
│   └── audit/
├── templates/
├── static/
└── manage.py
```

## Initial files
- `requirements.txt`: Django/DRF/PostgreSQL dependencies.
- `.env.example`: alle vereiste env vars.
- `.gitignore`: Python/Django, env, editor, logs.
- `README.md`: quickstart, docs-overzicht.

## Configuratievariabelen
- `DJANGO_SECRET_KEY`
- `DJANGO_DEBUG`
- `DJANGO_ALLOWED_HOSTS`
- `DATABASE_URL`
- `TIME_ZONE`
- `CSRF_TRUSTED_ORIGINS`
