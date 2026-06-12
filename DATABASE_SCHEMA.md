# DATABASE_SCHEMA.md

## ER Diagram (tekstueel)
- Camp 1---N Team
- Team 1---N Participant
- Camp 1---N Activity
- Team 1---N PointTransaction
- Activity 1---N PointTransaction
- User 1---N PointTransaction
- Camp 1---N AuditLog

## SQL Schema (PostgreSQL)
```sql
CREATE TABLE camps (
  id BIGSERIAL PRIMARY KEY,
  name VARCHAR(120) NOT NULL,
  start_date DATE NOT NULL,
  end_date DATE NOT NULL,
  login_code VARCHAR(32) NOT NULL UNIQUE,
  is_archived BOOLEAN NOT NULL DEFAULT FALSE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  CHECK (end_date >= start_date)
);

CREATE TABLE teams (
  id BIGSERIAL PRIMARY KEY,
  camp_id BIGINT NOT NULL REFERENCES camps(id) ON DELETE CASCADE,
  name VARCHAR(120) NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  UNIQUE(camp_id, name)
);

CREATE TABLE participants (
  id BIGSERIAL PRIMARY KEY,
  team_id BIGINT NOT NULL REFERENCES teams(id) ON DELETE CASCADE,
  name VARCHAR(120) NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  UNIQUE(team_id, name)
);

CREATE TABLE activities (
  id BIGSERIAL PRIMARY KEY,
  camp_id BIGINT REFERENCES camps(id) ON DELETE CASCADE,
  name VARCHAR(120) NOT NULL,
  default_points SMALLINT NOT NULL,
  category VARCHAR(20) NOT NULL,
  is_global BOOLEAN NOT NULL DEFAULT FALSE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  CHECK (category IN ('spel','activiteit','gedrag')),
  CHECK (default_points IN (-15,-10,-5,5,10,15,20,25,30,35,40,45,50))
);

CREATE TABLE app_users (
  id BIGSERIAL PRIMARY KEY,
  username VARCHAR(120) NOT NULL UNIQUE,
  role VARCHAR(20) NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  CHECK (role IN ('superadmin','leader'))
);

CREATE TABLE point_transactions (
  id BIGSERIAL PRIMARY KEY,
  timestamp TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  camp_id BIGINT NOT NULL REFERENCES camps(id) ON DELETE CASCADE,
  team_id BIGINT NOT NULL REFERENCES teams(id) ON DELETE CASCADE,
  amount SMALLINT NOT NULL,
  activity_id BIGINT REFERENCES activities(id) ON DELETE SET NULL,
  given_by_user_id BIGINT REFERENCES app_users(id) ON DELETE SET NULL,
  optional_person_name VARCHAR(120),
  note VARCHAR(255),
  correction_of_id BIGINT REFERENCES point_transactions(id) ON DELETE SET NULL,
  CHECK (amount IN (-15,-10,-5,5,10,15,20,25,30,35,40,45,50))
);

CREATE TABLE audit_logs (
  id BIGSERIAL PRIMARY KEY,
  camp_id BIGINT REFERENCES camps(id) ON DELETE CASCADE,
  actor_user_id BIGINT REFERENCES app_users(id) ON DELETE SET NULL,
  action VARCHAR(80) NOT NULL,
  entity_type VARCHAR(80) NOT NULL,
  entity_id BIGINT,
  payload JSONB NOT NULL DEFAULT '{}'::jsonb,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_team_camp ON teams(camp_id);
CREATE INDEX idx_tx_camp_timestamp ON point_transactions(camp_id, timestamp DESC);
CREATE INDEX idx_tx_team_timestamp ON point_transactions(team_id, timestamp DESC);
CREATE INDEX idx_audit_camp_created ON audit_logs(camp_id, created_at DESC);
```

## Migratievolgorde
1. camps
2. teams
3. participants
4. activities
5. app_users
6. point_transactions
7. audit_logs
