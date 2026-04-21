# FixMate Backend — Rails 8 API

The API backend for FixMate India, built with **Ruby on Rails 8** in API-only mode.

---

## Tech Stack

| Component        | Technology                          |
|------------------|-------------------------------------|
| Framework        | Ruby on Rails 8.0.4 (API mode)     |
| Ruby Version     | 3.2.2                               |
| Database         | PostgreSQL                          |
| Authentication   | Devise 5.0 + devise-jwt 0.13       |
| Password Hashing | bcrypt                              |
| Web Server       | Puma                                |
| File Uploads     | ActiveStorage                       |
| Email            | ActionMailer (letter_opener in dev) |
| Background Jobs  | Solid Queue                         |
| Caching          | Solid Cache                         |
| WebSocket        | Solid Cable                         |
| Deployment       | Docker + Kamal + Thruster           |

---

## Prerequisites

- **Ruby** 3.2.2 — install via [rbenv](https://github.com/rbenv/rbenv) or [rvm](https://rvm.io/)
- **PostgreSQL** ≥ 14 — [installation guide](https://www.postgresql.org/download/)
- **Bundler** — `gem install bundler`

---

## Installation

```bash
# 1. Navigate to the backend directory
cd backend

# 2. Install Ruby gems
bundle install

# 3. Create the PostgreSQL databases
rails db:create

# 4. Run all migrations
rails db:migrate

# 5. Seed the database with initial data
rails db:seed
```

### What the seed file creates:

| Entity       | Data                                                          |
|--------------|---------------------------------------------------------------|
| **Cities**   | Indore, Mumbai, Bhopal, Delhi, Pune                           |
| **Categories** | Electrician, Plumber, AC Repair                             |
| **Services** | Fan Repair (₹150), Motor Rewinding (₹800), Wiring Installation (₹500), Switchboard Installation (₹200), Pipe Leakage Repair (₹250), Bathroom Fitting (₹1500), Tap Replacement (₹150), AC Servicing (₹499), Gas Filling (₹1500) |

---

## Running the Server

```bash
# Start on port 3001 (frontend expects this port)
rails server -p 3001
```

The API will be available at **http://localhost:3001**

---

## Database Configuration

Database settings are in `config/database.yml`:

| Environment   | Database Name          |
|--------------|------------------------|
| Development  | `backend_development`  |
| Test         | `backend_test`         |
| Production   | `backend_production`   |

> **Note:** By default, Rails connects via Unix domain socket. If you need TCP, uncomment `host` and `port` in `database.yml`.

---

## Models

| Model            | Description                                    |
|------------------|------------------------------------------------|
| `User`           | Core user with email, name, role (customer/worker), Devise auth |
| `WorkerProfile`  | Extends User with experience, hourly rate, availability, city   |
| `City`           | Supported cities for worker search              |
| `Category`       | Service categories (Electrician, Plumber, etc.) |
| `Service`        | Individual services under a category            |
| `WorkerService`  | Join table linking workers to their services    |
| `JobRequest`     | Customer-to-worker job with status tracking     |
| `Notification`   | In-app notification with read/unread state      |
| `JwtDenylist`    | Revoked JWT tokens for secure logout            |

---

## API Endpoints

All endpoints are under `/api/v1`.

### Authentication (Devise)

```
POST   /api/v1/auth/register    → Create account
POST   /api/v1/auth/login       → Login, returns JWT in Authorization header
DELETE /api/v1/auth/logout       → Logout, invalidates JWT
```

### Users
```
GET    /api/v1/users/me          → Current user profile
```

### Workers
```
GET    /api/v1/workers           → List workers (filterable)
GET    /api/v1/workers/:id       → Worker details
GET    /api/v1/workers/profile   → Own worker profile (auth)
PUT    /api/v1/workers/profile   → Update profile (auth)
PATCH  /api/v1/workers/availability → Toggle availability (auth)
```

### Jobs
```
POST   /api/v1/jobs              → Create job request (customer)
GET    /api/v1/jobs/my           → My jobs (auth)
GET    /api/v1/jobs/:id          → Job details (auth)
PATCH  /api/v1/jobs/:id/accept   → Accept job (worker)
PATCH  /api/v1/jobs/:id/reject   → Reject job (worker)
PATCH  /api/v1/jobs/:id/complete → Complete job (worker)
PATCH  /api/v1/jobs/:id/cancel   → Cancel job (customer)
```

### Lookup Data
```
GET    /api/v1/cities            → All supported cities
GET    /api/v1/categories        → All service categories
```

### Notifications
```
GET    /api/v1/notifications              → User notifications
GET    /api/v1/notifications/unread_count  → Unread count
PATCH  /api/v1/notifications/:id/mark_read → Mark as read
```

---

## Email Notifications

The `JobMailer` sends email to workers when a new job request is created.

**In development**, emails open in the browser automatically via the `letter_opener` gem — no SMTP configuration needed.

---

## Docker

```bash
# Build the production image
docker build -t fixmate-backend .

# Run the container
docker run -d -p 80:80 \
  -e RAILS_MASTER_KEY=<value from config/master.key> \
  --name fixmate-backend \
  fixmate-backend
```

---

## Useful Commands

```bash
rails routes                    # View all routes
rails console                   # Interactive Rails console
rails db:reset                  # Drop, create, migrate, and seed
rails db:migrate:status         # Check migration status
bundle exec rubocop             # Run Ruby linter
bundle exec brakeman            # Security vulnerability scan
```

---

## Environment Variables

| Variable                     | Required | Description                       |
|------------------------------|:--------:|-----------------------------------|
| `RAILS_MASTER_KEY`          | Prod     | Master key for encrypted credentials |
| `BACKEND_DATABASE_PASSWORD` | Prod     | PostgreSQL password               |
| `RAILS_MAX_THREADS`        | No       | Thread pool size (default: 5)     |
| `RAILS_ENV`                | No       | Environment (default: development)|
