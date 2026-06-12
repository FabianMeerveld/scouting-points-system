# API_SPEC.md

Base URL: `/api/v1`

## Auth
### POST `/auth/camp-login`
Request:
```json
{ "login_code": "KAMP2026A" }
```
Response 200:
```json
{ "data": { "camp_id": 1, "camp_name": "Zomerkamp 2026" } }
```

### POST `/auth/logout`
Response 204

## Camps (admin)
- `GET /admin/camps`
- `POST /admin/camps`
- `GET /admin/camps/{id}`
- `PATCH /admin/camps/{id}`
- `POST /admin/camps/{id}/archive`

Create request:
```json
{
  "name": "Weekendkamp Mei",
  "start_date": "2026-05-15",
  "end_date": "2026-05-17",
  "login_code": "WKMEI26"
}
```

## Teams & deelnemers (admin)
- `POST /admin/camps/{camp_id}/teams`
- `PATCH /admin/teams/{team_id}`
- `POST /admin/teams/{team_id}/participants`
- `DELETE /admin/participants/{participant_id}`

## Activities
- `GET /camps/{camp_id}/activities`
- `POST /camps/{camp_id}/activities`

Create request:
```json
{
  "name": "Corvee uitstekend",
  "default_points": 10,
  "category": "gedrag"
}
```

## Transactions
### POST `/camps/{camp_id}/transactions`
Request:
```json
{
  "team_id": 3,
  "amount": 15,
  "activity_id": 9,
  "optional_person_name": "Sven",
  "note": "Hielp extra bij afwas"
}
```
Response 201:
```json
{
  "data": {
    "id": 501,
    "timestamp": "2026-07-22T14:05:12Z",
    "team_id": 3,
    "amount": 15
  }
}
```

### POST `/camps/{camp_id}/transactions/{id}/correct`
Request:
```json
{ "reason": "Verkeerde team geselecteerd" }
```
Response 201 (tegenboeking aangemaakt)

### GET `/camps/{camp_id}/transactions?since=2026-07-22T00:00:00Z&limit=50`

## Leaderboard
### GET `/camps/{camp_id}/leaderboard`
Response:
```json
{
  "data": [
    {
      "team_id": 2,
      "team_name": "Wolven",
      "total_score": 145,
      "today_score": 35
    }
  ],
  "meta": { "generated_at": "2026-07-22T14:06:00Z" }
}
```

### GET `/camps/{camp_id}/daily-feed`
Lijst transacties van vandaag/afgelopen 24 uur.

## Audit logs (admin)
### GET `/admin/camps/{camp_id}/audit-logs?from=2026-07-20&to=2026-07-22&page=1`

## Foutresponses
```json
{
  "errors": [
    { "code": "INVALID_POINTS", "message": "Allowed values: -15..-5 and 5..50 in steps of 5" }
  ]
}
```
