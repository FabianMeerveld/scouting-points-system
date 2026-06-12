# DEPLOYMENT_GUIDE.md

## Doelomgeving
- Raspberry Pi 4/5 met Raspberry Pi OS (64-bit)
- Domein: `http://raspberrypi.local`
- Python 3.12, PostgreSQL 16, Nginx, Gunicorn

## 1. Systeem packages
```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-pip postgresql postgresql-contrib nginx
```

## 2. PostgreSQL setup
```bash
sudo -u postgres psql
CREATE DATABASE scouting_points;
CREATE USER scouting_app WITH PASSWORD 'change_me_strong';
GRANT ALL PRIVILEGES ON DATABASE scouting_points TO scouting_app;
\q
```

## 3. App setup
```bash
cd /opt
sudo git clone <repo-url> scouting-points-system
cd scouting-points-system
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

## 4. Django setup
```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py collectstatic --noinput
```

## 5. Gunicorn systemd service
Maak `/etc/systemd/system/scouting-points.service`:
```ini
[Unit]
Description=Scouting Points Django App
After=network.target

[Service]
User=pi
Group=www-data
WorkingDirectory=/opt/scouting-points-system
EnvironmentFile=/opt/scouting-points-system/.env
ExecStart=/opt/scouting-points-system/.venv/bin/gunicorn config.wsgi:application --bind 127.0.0.1:8000
Restart=always

[Install]
WantedBy=multi-user.target
```

## 6. Nginx reverse proxy
Maak `/etc/nginx/sites-available/scouting-points` en link naar `sites-enabled`:
```nginx
server {
    listen 80;
    server_name raspberrypi.local;

    location /static/ {
        alias /opt/scouting-points-system/staticfiles/;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 7. Start services
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now scouting-points.service
sudo nginx -t && sudo systemctl reload nginx
```

## 8. Backup advies
- Dagelijkse `pg_dump` via cron naar externe schijf/NAS.
- Minimaal 7 dagelijkse backups bewaren.
