# Architecture Document
## CricBuzz Clone - Technical Architecture

**Document Version:** 1.0  
**Last Updated:** 2026-09-12  
**Architecture Owner:** TBD

---

## 1. Architecture Overview

This document describes the technical architecture of the CricBuzz Clone application, a web-based cricket score tracking platform with real-time updates, user authentication, and personalization features.

### 1.1 Architecture Type
**Layered Architecture with Microservices-Ready Design**
- Separation of concerns (Frontend, Backend, Data Layer)
- Horizontally scalable backend
- Real-time communication via WebSocket
- Caching layer for performance

### 1.2 System Components
```
┌─────────────────────────────────────────────────────┐
│                   CLIENT TIER                        │
│  (React SPA - Browser, Mobile Web)                   │
└────────────────┬────────────────────────────────────┘
                 │
         ┌───────┴────────┐
         │                │
      HTTP/REST        WebSocket
         │                │
         └───────┬────────┘
                 │
         ┌───────▼────────────────────────────────┐
         │      API GATEWAY (Reverse Proxy)       │
         │   (Nginx/Node.js Express Gateway)      │
         └───────┬────────────────────────────────┘
                 │
┌────────────────┴──────────────────────────────────┐
│              APPLICATION TIER                      │
│         (Python - FastAPI/Django)                  │
│                                                    │
│  ┌──────────────┐  ┌──────────────┐             │
│  │  Auth        │  │  Match        │             │
│  │  Service     │  │  Service      │             │
│  └──────────────┘  └──────────────┘             │
│  ┌──────────────┐  ┌──────────────┐             │
│  │  User        │  │  Notification│             │
│  │  Service     │  │  Service      │             │
│  └──────────────┘  └──────────────┘             │
│  ┌──────────────┐  ┌──────────────┐             │
│  │  Search      │  │  Player Stats│             │
│  │  Service     │  │  Service      │             │
│  └──────────────┘  └──────────────┘             │
└────────────────┬──────────────────────────────────┘
                 │
         ┌───────┴──────────────┐
         │                      │
      ┌──▼──────────┐       ┌──▼──────────┐
      │  CACHE      │       │  DATABASE   │
      │  (Redis)    │       │  (PostgSQL) │
      └─────────────┘       └─────────────┘
         │
         └────────────────────┐
                              │
                    ┌─────────▼──────────┐
                    │  EXTERNAL API      │
                    │ (Sports Score API) │
                    └────────────────────┘
```

---

## 2. Technology Stack

### 2.1 Frontend
- **Framework:** React 18.x
- **Language:** JavaScript/TypeScript
- **State Management:** Redux Toolkit or Context API
- **HTTP Client:** Axios
- **WebSocket Client:** Socket.io-client or ws
- **Styling:** Tailwind CSS / CSS Modules
- **Build Tool:** Vite or Create React App
- **Package Manager:** npm or yarn
- **Testing:** Jest + React Testing Library
- **Linting:** ESLint + Prettier

### 2.2 Backend
- **Language:** Python 3.9+
- **Web Framework:** FastAPI (recommended) or Django
- **ASGI Server:** Uvicorn (for FastAPI) or Gunicorn (for Django)
- **Package Manager:** pip with virtual environment
- **Testing:** pytest
- **API Documentation:** FastAPI Auto Docs or Swagger
- **Authentication:** PyJWT for JWT tokens, bcrypt for passwords

### 2.3 Database
- **Primary Database:** PostgreSQL 12+
  - User accounts and profiles
  - Favorites (matches, teams, players)
  - Notifications history
  - Match metadata (non-live)
- **Cache Database:** Redis 6+
  - Live match scores cache
  - Session tokens cache
  - Rate limiting data
  - Real-time notifications queue

### 2.4 Infrastructure
- **Web Server:** Nginx (reverse proxy, load balancer)
- **Process Manager:** Supervisor or systemd
- **Containerization:** Docker (optional, for easier deployment)
- **Monitoring:** TBD (could be basic logging initially)
- **Logging:** Python logging + file rotation

---

## 3. Detailed Architecture Layers

### 3.1 Presentation Layer (Frontend)

#### 3.1.1 Components Structure
```
src/
├── components/
│   ├── Header/
│   ├── Navigation/
│   ├── Match/
│   │   ├── MatchCard.jsx
│   │   ├── MatchDetail.jsx
│   │   ├── Scorecard.jsx
│   │   └── Commentary.jsx
│   ├── Player/
│   ├── Team/
│   ├── User/
│   ├── Notifications/
│   └── Common/
├── pages/
│   ├── Home.jsx
│   ├── MatchDetail.jsx
│   ├── PlayerProfile.jsx
│   ├── TeamProfile.jsx
│   ├── SearchResults.jsx
│   ├── UserProfile.jsx
│   ├── Login.jsx
│   ├── Register.jsx
│   └── 404.jsx
├── services/
│   ├── api.js (Axios instance)
│   ├── matchService.js
│   ├── authService.js
│   ├── userService.js
│   └── notificationService.js
├── store/
│   ├── slices/
│   │   ├── authSlice.js
│   │   ├── matchSlice.js
│   │   ├── userSlice.js
│   │   ├── notificationSlice.js
│   │   └── uiSlice.js
│   └── store.js
├── hooks/
│   ├── useAuth.js
│   ├── useMatches.js
│   ├── useWebSocket.js
│   └── useNotifications.js
├── utils/
│   ├── dateFormatter.js
│   ├── scoreCalculator.js
│   ├── validators.js
│   └── constants.js
├── styles/
│   └── globals.css
└── App.jsx
```

#### 3.1.2 Key Features
- **Responsive Design:** Mobile-first, works on all screen sizes
- **Real-time Updates:** WebSocket connection for live scores
- **State Management:** Redux for global state (matches, user, notifications)
- **Authentication:** JWT tokens stored in localStorage/sessionStorage
- **Caching:** Service workers for offline capability (future)
- **Error Handling:** Global error boundary and axios interceptors

### 3.2 API Layer (Backend)

#### 3.2.1 API Structure
```
api/
├── __init__.py
├── main.py (FastAPI app entry point)
├── config.py (Configuration, environment variables)
├── middleware/
│   ├── __init__.py
│   ├── auth.py (JWT middleware)
│   ├── error_handler.py
│   ├── rate_limiter.py
│   └── cors.py
├── routes/
│   ├── __init__.py
│   ├── auth.py (Login, Register, Logout endpoints)
│   ├── matches.py (Match endpoints)
│   ├── players.py (Player endpoints)
│   ├── teams.py (Team endpoints)
│   ├── users.py (User profile endpoints)
│   ├── favorites.py (Favorites endpoints)
│   └── search.py (Search endpoints)
├── services/
│   ├── __init__.py
│   ├── auth_service.py (Authentication logic)
│   ├── match_service.py (Match business logic)
│   ├── player_service.py (Player statistics)
│   ├── team_service.py (Team information)
│   ├── user_service.py (User management)
│   ├── notification_service.py (Notification logic)
│   ├── sports_api_service.py (Integration with Sports Score API)
│   └── cache_service.py (Redis operations)
├── models/
│   ├── __init__.py
│   ├── user.py (User model/schema)
│   ├── match.py (Match model/schema)
│   ├── player.py (Player model/schema)
│   ├── team.py (Team model/schema)
│   ├── favorite.py (Favorite model/schema)
│   └── notification.py (Notification model/schema)
├── database/
│   ├── __init__.py
│   ├── session.py (Database session management)
│   ├── models.py (SQLAlchemy ORM models)
│   └── migrations/ (Alembic migrations)
├── tasks/
│   ├── __init__.py
│   ├── score_updater.py (Background task for score updates)
│   ├── notification_worker.py (Notification processing)
│   └── scheduled_tasks.py (Scheduled jobs)
├── utils/
│   ├── __init__.py
│   ├── validators.py
│   ├── helpers.py
│   ├── exceptions.py
│   └── constants.py
├── websocket/
│   ├── __init__.py
│   ├── manager.py (WebSocket connection management)
│   └── handlers.py (WebSocket message handlers)
└── tests/
    ├── __init__.py
    ├── test_auth.py
    ├── test_matches.py
    ├── test_players.py
    ├── test_users.py
    └── test_integration.py
```

#### 3.2.2 API Endpoints

**Authentication Endpoints:**
```
POST   /api/auth/register          - User registration
POST   /api/auth/login             - User login
POST   /api/auth/logout            - User logout
POST   /api/auth/refresh-token     - Refresh JWT token
GET    /api/auth/verify            - Verify token validity
```

**Match Endpoints:**
```
GET    /api/matches                - List all matches (with filters)
GET    /api/matches/:id            - Get match details
GET    /api/matches/live           - Get live matches only
GET    /api/matches/upcoming       - Get upcoming matches
GET    /api/matches/:id/scorecard  - Get detailed scorecard
GET    /api/matches/:id/commentary - Get ball-by-ball commentary
```

**Player Endpoints:**
```
GET    /api/players                - Search players
GET    /api/players/:id            - Get player profile
GET    /api/players/:id/stats      - Get player statistics
GET    /api/players/:id/recent     - Get recent performance
```

**Team Endpoints:**
```
GET    /api/teams                  - List all teams
GET    /api/teams/:id              - Get team details
GET    /api/teams/:id/squad        - Get team squad
GET    /api/teams/:id/stats        - Get team statistics
```

**User Endpoints:**
```
GET    /api/users/profile          - Get current user profile
PUT    /api/users/profile          - Update user profile
GET    /api/users/favorites        - Get user favorites
POST   /api/users/favorites/:type/:id - Add favorite
DELETE /api/users/favorites/:type/:id - Remove favorite
```

**Notification Endpoints:**
```
GET    /api/notifications          - Get user notifications
PUT    /api/notifications/:id/read - Mark notification as read
PUT    /api/notifications/settings - Update notification preferences
```

**Search Endpoints:**
```
GET    /api/search                 - Global search (matches, teams, players)
GET    /api/search/matches         - Search matches
GET    /api/search/teams           - Search teams
GET    /api/search/players         - Search players
```

### 3.3 Business Logic Layer

#### 3.3.1 Services Description

**AuthService**
- User registration with validation
- User login with password verification
- JWT token generation and validation
- Token refresh mechanism
- Password reset flow

**MatchService**
- Fetch match data from Sports Score API
- Cache live match scores
- Format match data for display
- Update match status
- Calculate derived metrics

**PlayerService**
- Fetch player statistics from Sports Score API
- Cache player profiles
- Search players by name/country
- Calculate career statistics
- Get recent performance

**UserService**
- User profile management
- Password hashing and verification
- User preferences management
- Account deletion

**NotificationService**
- Send in-app notifications
- Store notification history
- Browser push notifications
- Notification preferences management
- Scheduled notifications

**CacheService**
- Redis operations
- Cache invalidation strategies
- TTL management
- Cache warming

### 3.4 Data Layer

#### 3.4.1 Database Schema

**Users Table**
```sql
users
├── id (PK)
├── email (UNIQUE)
├── password_hash
├── first_name
├── last_name
├── country
├── created_at
├── updated_at
├── last_login
└── is_active
```

**Favorites Table**
```sql
favorites
├── id (PK)
├── user_id (FK)
├── favorite_type (match/team/player)
├── entity_id
├── created_at
└── updated_at
```

**Match_Cache Table** (for storing frequently accessed matches)
```sql
match_cache
├── id (PK)
├── api_match_id (UNIQUE)
├── match_data (JSON)
├── last_updated
├── status
└── TTL
```

**Notifications Table**
```sql
notifications
├── id (PK)
├── user_id (FK)
├── title
├── message
├── type (score_milestone/wicket/match_start/etc)
├── entity_id (match_id/team_id/etc)
├── is_read
├── created_at
└── read_at
```

**NotificationPreferences Table**
```sql
notification_preferences
├── id (PK)
├── user_id (FK, UNIQUE)
├── enable_score_updates
├── enable_wicket_alerts
├── enable_match_start
├── enable_email
├── enable_push
├── updated_at
```

#### 3.4.2 Redis Cache Strategy

**Live Match Scores** (TTL: 10 seconds)
```
Key: match:{match_id}
Value: {
  "match_id": "...",
  "status": "live",
  "runs": 150,
  "wickets": 3,
  "overs": 25.3,
  "updated_at": timestamp
}
```

**User Sessions** (TTL: 24 hours)
```
Key: session:{token_hash}
Value: {
  "user_id": "...",
  "email": "...",
  "issued_at": timestamp,
  "expires_at": timestamp
}
```

**Notification Queue** (Processed by workers)
```
Queue: notifications:pending
Messages: {
  "user_id": "...",
  "type": "wicket_alert",
  "match_id": "...",
  "message": "..."
}
```

---

## 4. Communication Patterns

### 4.1 Synchronous Communication

**REST API (HTTP/HTTPS)**
- Client to Backend: Standard REST endpoints
- Response Format: JSON
- Authentication: Bearer Token (JWT) in Authorization header
- Error Response Format:
```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Detailed error message",
    "status": 400
  }
}
```

### 4.2 Asynchronous Communication

**WebSocket (Real-time Updates)**
- Protocol: WebSocket over HTTPS (wss://)
- Client connects to `/ws/matches/{match_id}`
- Server broadcasts score updates every 5-10 seconds
- Message Format:
```json
{
  "type": "score_update",
  "match_id": "...",
  "data": {
    "runs": 150,
    "wickets": 3,
    "overs": 25.3,
    "timestamp": 1234567890
  }
}
```

**Background Tasks (Celery/APScheduler)**
- Score fetching from Sports Score API (every 10 seconds for live matches)
- Notification sending to users
- Cache refresh
- Database maintenance

---

## 5. Security Architecture

### 5.1 Authentication & Authorization

**JWT Token Flow:**
1. User login → Credentials validated
2. JWT token generated with user claims
3. Token stored in browser localStorage
4. Token sent in Authorization header: `Bearer <token>`
5. Backend validates token and extracts user info
6. Access granted to protected resources

**Token Structure:**
```
Header: { "alg": "HS256", "typ": "JWT" }
Payload: {
  "user_id": "...",
  "email": "...",
  "exp": timestamp,
  "iat": timestamp
}
Signature: HMACSHA256(header.payload, secret)
```

### 5.2 Password Security
- Passwords hashed with bcrypt (salt rounds: 10)
- Passwords never logged or transmitted in plain text
- Password reset via email verification link

### 5.3 API Security
- **CORS:** Restrict to allowed origins
- **Rate Limiting:** Limit API calls per IP/user
- **Input Validation:** All inputs validated and sanitized
- **SQL Injection Prevention:** Parameterized queries via ORM
- **XSS Prevention:** React's built-in protection, CSP headers
- **HTTPS:** All traffic encrypted

### 5.4 Data Protection
- Sensitive data (passwords, tokens) never logged
- Database backups encrypted
- Secrets stored in environment variables
- No hardcoded credentials in code

---

## 6. Performance & Scalability

### 6.1 Caching Strategy

**Multi-Level Caching:**
1. **Browser Cache:** Static assets (CSS, JS, images)
2. **Redis Cache:** Live scores, player stats, team info
3. **Database Query Cache:** Frequently accessed data
4. **API Response Cache:** External API responses

**Cache Invalidation:**
- TTL-based: Automatic expiry
- Event-based: Invalidate on data update
- Manual: Admin cache clear command

### 6.2 Database Optimization

**Indexing Strategy:**
- Index on user_id for faster lookups
- Index on entity_id for favorites search
- Index on timestamp for time-range queries
- Composite indexes for common filter combinations

**Query Optimization:**
- Connection pooling
- Query result pagination
- Lazy loading relationships
- Avoid N+1 queries

### 6.3 Load Balancing

**Multiple Backend Instances:**
```
Nginx (Load Balancer)
├── Backend Instance 1 (FastAPI)
├── Backend Instance 2 (FastAPI)
├── Backend Instance 3 (FastAPI)
└── Backend Instance 4 (FastAPI)
```

**WebSocket Load Balancing:**
- Sticky sessions (same user to same backend)
- Redis pub/sub for cross-instance communication

### 6.4 Horizontal Scaling

**Scale-out Approach:**
- Add more backend instances
- Add more Redis replicas
- Add PostgreSQL read replicas
- Use CDN for static assets

---

## 7. Deployment Architecture

### 7.1 Local Development Setup
```
Developer Machine
├── Frontend (React, npm start)
├── Backend (Python, uvicorn)
├── PostgreSQL (Docker or local)
└── Redis (Docker or local)
```

### 7.2 Local Deployment (Server)
```
Production Server
├── Nginx (Reverse Proxy on :80, :443)
├── Backend (Uvicorn on :8000)
├── PostgreSQL (Default port :5432)
├── Redis (Default port :6379)
└── Supervisor/Systemd (Process management)
```

### 7.3 Directory Structure (Server)
```
/opt/cricbuzz/
├── frontend/
│   ├── build/
│   └── package.json
├── backend/
│   ├── venv/
│   ├── app/
│   ├── requirements.txt
│   └── .env
├── config/
│   ├── nginx.conf
│   ├── supervisor.conf
│   └── systemd/
├── logs/
│   ├── nginx.log
│   ├── backend.log
│   └── redis.log
└── scripts/
    ├── deploy.sh
    ├── backup.sh
    └── health_check.sh
```

---

## 8. Monitoring & Logging

### 8.1 Logging Strategy

**Application Logging:**
- Python logging module with file rotation
- Log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
- Logs stored in `/var/log/cricbuzz/`
- Structured logging format (JSON)

**Log Retention:**
- Daily log rotation
- Keep 7 days of logs
- Archive older logs

### 8.2 Monitoring Checklist
- [ ] Backend process status (Supervisor/Systemd)
- [ ] Database connectivity
- [ ] Redis connectivity
- [ ] External API availability (Sports Score API)
- [ ] Disk space usage
- [ ] Memory usage
- [ ] CPU usage
- [ ] API response times
- [ ] Error rates

---

## 9. Disaster Recovery & Backup

### 9.1 Backup Strategy
- **Database Backups:** Daily at 2 AM, retain 7 days
- **Backup Location:** Separate storage location
- **Backup Verification:** Test restore monthly

### 9.2 Recovery Procedure
1. Restore PostgreSQL from backup
2. Clear Redis cache
3. Restart backend services
4. Verify API endpoints
5. Test user login flow

---

## 10. Development Workflow

### 10.1 Version Control
- GitHub repository
- Branch strategy: main (production), develop (development), feature branches
- Pull requests required for code review

### 10.2 Deployment Pipeline
```
Code Commit → GitHub → CI/CD (if using) → Testing → Deployment
```

### 10.3 Environment Variables (Backend .env)
```
FLASK_ENV=production
SECRET_KEY=<random-secret-key>
DATABASE_URL=postgresql://user:password@localhost/cricbuzz
REDIS_URL=redis://localhost:6379/0
SPORTS_API_KEY=<sports-score-api-key>
SPORTS_API_URL=https://api.sportscore.com
JWT_SECRET=<jwt-secret-key>
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24
CORS_ORIGINS=http://localhost:3000,https://example.com
DEBUG=False
```

---

## 11. API Documentation

- **Frontend API Docs:** Available at `/api/docs` (Swagger UI)
- **Backend Docs:** README in backend folder
- **Database Schema:** Database ERD and migration files

---

## 12. Future Scalability Considerations

1. **Microservices:** Split services into separate deployments
2. **Message Queue:** RabbitMQ or Kafka for async processing
3. **Search Engine:** Elasticsearch for advanced search
4. **API Gateway:** Kong or AWS API Gateway
5. **CDN:** CloudFlare or AWS CloudFront for static assets
6. **Container Orchestration:** Kubernetes for auto-scaling
7. **GraphQL:** Consider GraphQL for complex data queries
8. **Event Sourcing:** Store all events for audit trail

---

## 13. Assumptions & Constraints

### 13.1 Assumptions
- Single developer team (scalable to multiple developers)
- Local server deployment initially
- Sports Score API reliable for data
- PostgreSQL can handle expected load
- WebSocket connections limited to 1000 concurrent

### 13.2 Constraints
- No budget for cloud services
- Local hosting only
- Single server deployment (no redundancy initially)
- Manual backup and recovery procedures

---

## 14. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-09-12 | Architecture Team | Initial architecture design |

---

**Document Owner:** TBD  
**Last Review Date:** 2026-09-12  
**Next Review Date:** 2026-10-12

