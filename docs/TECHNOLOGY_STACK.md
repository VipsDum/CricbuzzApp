# Technology Stack & Dependencies
## CricBuzz Clone - Complete Package List

**Document Version:** 1.0  
**Date Created:** 2026-09-12  
**Last Updated:** 2026-09-12

---

## 1. Overview

This document provides a comprehensive list of all technologies, frameworks, libraries, and packages used in the CricBuzz Clone project. It includes version information, purpose, and justification for each choice.

### 1.1 Technology Stack Summary

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | React 18 + TypeScript | UI framework |
| **Frontend Build** | Vite | Build tool |
| **State Management** | Redux Toolkit | Global state management |
| **HTTP Client** | Axios | API calls |
| **WebSocket** | Socket.io-client | Real-time updates |
| **Styling** | Tailwind CSS | Utility-first CSS |
| **Backend** | FastAPI | API framework |
| **ASGI Server** | Uvicorn | Application server |
| **ORM** | SQLAlchemy | Database layer |
| **Database** | PostgreSQL | Primary database |
| **Cache** | Redis | In-memory cache |
| **Authentication** | PyJWT + bcrypt | Auth tokens & passwords |
| **Task Scheduler** | APScheduler | Background tasks |
| **Testing** | pytest + Jest | Unit & integration tests |
| **Deployment** | Nginx + Supervisor | Production setup |

---

## 2. Frontend Dependencies

### 2.1 Core Framework

#### React & React DOM
```json
{
  "react": "^18.2.0",
  "react-dom": "^18.2.0"
}
```
- **Purpose:** UI library and DOM rendering
- **Why:** Industry standard, large ecosystem, excellent documentation
- **Alternatives considered:** Vue.js (simpler but smaller ecosystem), Svelte (compiler-based)

---

### 2.2 State Management

#### Redux & Redux Toolkit
```json
{
  "redux": "^4.2.1",
  "@reduxjs/toolkit": "^1.9.5",
  "react-redux": "^8.1.2"
}
```
- **Purpose:** Global state management for app data
- **Why:** Mature, predictable, developer tools, time-travel debugging
- **Slices needed:**
  - `authSlice` - User authentication state
  - `matchSlice` - Match data and filters
  - `userSlice` - User profile and preferences
  - `notificationSlice` - Notifications
  - `uiSlice` - UI state (loading, modals, theme)

#### Redux Persist (Optional)
```json
{
  "redux-persist": "^6.0.0"
}
```
- **Purpose:** Persist Redux state to localStorage
- **Why:** Maintain user state across page refresh
- **When to use:** Store user preferences, last viewed match

---

### 2.3 HTTP & API Communication

#### Axios
```json
{
  "axios": "^1.4.0"
}
```
- **Purpose:** HTTP client for API calls
- **Why:** Promise-based, automatic JSON transformation, interceptors
- **Features needed:**
  - Request interceptors (add JWT token to headers)
  - Response interceptors (handle token refresh, global error handling)
  - Timeout configuration
  - Base URL configuration

---

### 2.4 Real-time Communication

#### Socket.io-client
```json
{
  "socket.io-client": "^4.7.0"
}
```
- **Purpose:** WebSocket communication for real-time updates
- **Why:** Fallback to polling if WebSocket not available, automatic reconnection
- **Features needed:**
  - Connect to match score updates
  - Subscribe to notifications
  - Emit events (add favorite, etc.)
  - Handle disconnections

**Alternative:** `ws` (lightweight WebSocket library)
```json
{
  "ws": "^8.13.0"
}
```

---

### 2.5 Styling

#### Tailwind CSS
```json
{
  "tailwindcss": "^3.3.0",
  "postcss": "^8.4.24",
  "autoprefixer": "^10.4.14"
}
```
- **Purpose:** Utility-first CSS framework
- **Why:** Fast development, consistent design, small bundle size, responsive utilities
- **Configuration:** Custom theme with cricket-related colors

#### Tailwind UI Components (Optional)
```json
{
  "@headlessui/react": "^1.7.14",
  "@heroicons/react": "^2.0.18"
}
```
- **Purpose:** Accessible unstyled components and SVG icons
- **Why:** Pre-built, accessible, consistent with Tailwind

---

### 2.6 Form Handling

#### React Hook Form
```json
{
  "react-hook-form": "^7.45.2"
}
```
- **Purpose:** Performant, flexible form validation
- **Why:** Minimal re-renders, small bundle size, good DX
- **Forms needed:**
  - Login form
  - Register form
  - Update profile form
  - Search filters

#### Zod (Schema Validation)
```json
{
  "zod": "^3.22.2"
}
```
- **Purpose:** TypeScript-first schema validation
- **Why:** Type-safe, composable, good error messages
- **Alternative:** Yup (more mature but larger)

---

### 2.7 Routing

#### React Router
```json
{
  "react-router-dom": "^6.14.1"
}
```
- **Purpose:** Client-side routing and navigation
- **Why:** Standard library for React SPAs, handles nested routes
- **Routes needed:**
  - `/` - Home
  - `/login` - Login page
  - `/register` - Register page
  - `/matches/:id` - Match detail
  - `/players/:id` - Player profile
  - `/teams/:id` - Team profile
  - `/profile` - User profile (protected)
  - `/favorites` - User favorites (protected)
  - `/404` - 404 page

---

### 2.8 Date & Time

#### date-fns
```json
{
  "date-fns": "^2.30.0"
}
```
- **Purpose:** Date manipulation and formatting
- **Why:** Small, modular, immutable (unlike Moment.js)
- **Uses:**
  - Format match times
  - Calculate time remaining
  - Format notifications timestamps
  - Relative time (e.g., "2 hours ago")

**Alternative:** `Day.js` (even smaller)

---

### 2.9 HTTP Status & Utilities

#### HTTP-Status-Codes (Optional)
```json
{
  "http-status-codes": "^2.3.0"
}
```
- **Purpose:** HTTP status code constants
- **Why:** Better readability, less error-prone

---

### 2.10 Notifications & Toasts

#### React Toastify
```json
{
  "react-toastify": "^9.1.3"
}
```
- **Purpose:** Toast notifications UI
- **Why:** Easy to use, customizable, auto-dismiss, stacking
- **Use cases:**
  - Success messages (added favorite, logged in)
  - Error messages (API failures)
  - Info messages (score updates)
  - Warnings (unsaved changes)

**Alternative:** `notistack` or custom implementation

---

### 2.11 Testing

#### Jest
```json
{
  "jest": "^29.6.2",
  "@testing-library/react": "^14.0.0",
  "@testing-library/jest-dom": "^5.16.5",
  "@testing-library/user-event": "^14.4.3"
}
```
- **Purpose:** Unit and integration testing
- **Why:** Default in CRA, great ecosystem, good documentation
- **What to test:**
  - Component rendering
  - User interactions
  - Redux actions and reducers
  - API integration
  - Form validation

#### Vitest (Alternative)
```json
{
  "vitest": "^0.34.0"
}
```
- **Purpose:** Faster Jest alternative for Vite
- **Why:** Faster, better DX with Vite

---

### 2.12 Linting & Code Quality

#### ESLint
```json
{
  "eslint": "^8.45.0",
  "eslint-config-react-app": "^7.0.1"
}
```
- **Purpose:** Code linting and quality enforcement
- **Why:** Catch common mistakes, enforce code style
- **Plugins:**
  - eslint-plugin-react
  - eslint-plugin-react-hooks
  - eslint-plugin-jsx-a11y

#### Prettier
```json
{
  "prettier": "^3.0.0"
}
```
- **Purpose:** Code formatting
- **Why:** Consistent code style, no debate about formatting

---

### 2.13 Build Tools

#### Vite
```json
{
  "vite": "^4.4.9",
  "@vitejs/plugin-react": "^4.0.3"
}
```
- **Purpose:** Frontend build tool and dev server
- **Why:** Fast HMR, small bundle, modern JavaScript support
- **Alternative:** Create React App (simpler but less control)

---

### 2.14 TypeScript (Recommended)

#### TypeScript
```json
{
  "typescript": "^5.1.6"
}
```
- **Purpose:** Type safety for JavaScript
- **Why:** Catch errors early, better IDE support, self-documenting code
- **Usage:** Optional for MVP, but recommended for quality

---

### 2.15 Dev Dependencies Summary

```json
{
  "devDependencies": {
    "vite": "^4.4.9",
    "@vitejs/plugin-react": "^4.0.3",
    "typescript": "^5.1.6",
    "@types/react": "^18.2.14",
    "@types/react-dom": "^18.2.6",
    "@types/node": "^20.3.1",
    "eslint": "^8.45.0",
    "eslint-config-react-app": "^7.0.1",
    "prettier": "^3.0.0",
    "jest": "^29.6.2",
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/user-event": "^14.4.3"
  }
}
```

---

## 3. Backend Dependencies

### 3.1 Core Framework

#### FastAPI
```
fastapi==0.103.0
```
- **Purpose:** Python web framework for building APIs
- **Why:** Modern, fast, automatic API documentation, async support
- **Alternative:** Django (more batteries-included but slower for APIs)

#### Uvicorn
```
uvicorn==0.23.2
```
- **Purpose:** ASGI server to run FastAPI
- **Why:** Fast, supports async/await, production-ready
- **Installation:** `pip install uvicorn[standard]` (includes performance extras)

---

### 3.2 Database & ORM

#### SQLAlchemy
```
sqlalchemy==2.0.20
psycopg2-binary==2.9.7
```
- **Purpose:** Object-relational mapping (ORM) for database operations
- **Why:** Powerful, flexible, supports complex queries
- **Features:**
  - Define models as Python classes
  - Automatic migrations with Alembic
  - Query builder
  - Relationship management

#### Alembic
```
alembic==1.11.3
```
- **Purpose:** Database migration tool
- **Why:** Version control for database schema changes
- **Commands:**
  - `alembic init` - Initialize migration folder
  - `alembic revision --autogenerate -m "description"` - Create migration
  - `alembic upgrade head` - Apply migrations
  - `alembic downgrade -1` - Rollback migration

---

### 3.3 Authentication & Security

#### PyJWT
```
pyjwt==2.8.0
```
- **Purpose:** Create and verify JWT tokens
- **Why:** Standard, simple, good for stateless authentication
- **Usage:**
  - Generate token on login: `jwt.encode(payload, secret, algorithm="HS256")`
  - Verify token: `jwt.decode(token, secret, algorithms=["HS256"])`

#### Bcrypt
```
bcrypt==4.0.1
```
- **Purpose:** Password hashing and verification
- **Why:** Industry standard, resistant to GPU attacks, slow-by-design
- **Usage:**
  - Hash password: `bcrypt.hashpw(password.encode(), bcrypt.gensalt())`
  - Verify: `bcrypt.checkpw(password.encode(), hashed_password)`

#### Python-dotenv
```
python-dotenv==1.0.0
```
- **Purpose:** Load environment variables from .env file
- **Why:** Manage secrets and configuration without hardcoding
- **Usage:** `from dotenv import load_dotenv` then `os.getenv("VAR_NAME")`

#### CORS (Already in FastAPI)
```
fastapi-cors==0.0.6  # or use FastAPI's built-in
```
- **Purpose:** Handle Cross-Origin Resource Sharing
- **Why:** Allow frontend to call backend API from different domain/port

---

### 3.4 Caching

#### Redis-py
```
redis==5.0.0
```
- **Purpose:** Python client for Redis caching
- **Why:** In-memory data store, fast, perfect for caching
- **Usage:**
  - Cache live match scores
  - Session token storage
  - Rate limiting data
  - Notification queue

---

### 3.5 External API Integration

#### Requests
```
requests==2.31.0
```
- **Purpose:** HTTP library for API calls
- **Why:** Simple, reliable, handles retries well
- **Usage:** Call Sports Score API to fetch match data

#### HTTPx (Alternative)
```
httpx==0.24.1
```
- **Purpose:** Modern HTTP client with async support
- **Why:** async/await support, better for high-concurrency scenarios

---

### 3.6 WebSocket Support

#### python-socketio
```
python-socketio==5.9.0
python-engineio==4.7.1
```
- **Purpose:** WebSocket server for real-time communication
- **Why:** Fallback to polling if WebSocket not available, namespace support
- **Alternative:** FastAPI's built-in WebSocket support (simpler but less features)

---

### 3.7 Background Tasks & Scheduling

#### APScheduler
```
apscheduler==3.10.4
```
- **Purpose:** Schedule background tasks (fetch scores, send notifications)
- **Why:** Cron-like scheduling, in-process (no extra service needed for MVP)
- **Usage:**
  - Schedule score fetcher to run every 10 seconds for live matches
  - Schedule notification processor
  - Schedule cache refresh

#### Celery (Alternative)
```
celery==5.3.1
redis==5.0.0  # Celery broker
```
- **Purpose:** Distributed task queue
- **Why:** Better for high-scale, distributed systems
- **When to use:** Phase 2+ when scaling

---

### 3.8 Validation

#### Pydantic
```
pydantic==2.0.3
```
- **Purpose:** Data validation using Python type annotations
- **Why:** Built into FastAPI, fast validation, good error messages
- **Usage:**
  - Define request/response models
  - Automatic JSON schema generation
  - Type checking
  - Example:
    ```python
    class LoginRequest(BaseModel):
        email: str
        password: str
    ```

---

### 3.9 Testing

#### Pytest
```
pytest==7.4.0
pytest-asyncio==0.21.1
pytest-cov==4.1.0
```
- **Purpose:** Testing framework
- **Why:** Powerful fixtures, parametrization, great for async code
- **Packages:**
  - `pytest` - Main testing framework
  - `pytest-asyncio` - Support for async/await in tests
  - `pytest-cov` - Code coverage measurement

#### Httpx TestClient (for API testing)
```
# Already included with httpx
```
- **Purpose:** Test HTTP endpoints
- **Usage:** `TestClient(app)` to test FastAPI routes

---

### 3.10 Logging

#### Python Logging (Built-in)
- **Purpose:** Application logging
- **Why:** Built into Python, no extra dependency
- **Configuration:** Configure in config.py with file rotation

#### Loguru (Optional)
```
loguru==0.7.0
```
- **Purpose:** Better logging experience
- **Why:** Simpler API, automatic rotation, colored output
- **Alternative:** Python's standard logging

---

### 3.11 Environment & Configuration

#### Pydantic Settings
```
# Included with pydantic
```
- **Purpose:** Configuration management
- **Why:** Type-safe settings, environment variable support
- **Usage:**
  ```python
  class Settings(BaseSettings):
      database_url: str
      redis_url: str
      api_key: str
      class Config:
          env_file = ".env"
  ```

---

### 3.12 Utilities

#### Python-Dateutil
```
python-dateutil==2.8.2
```
- **Purpose:** Date and time utilities
- **Why:** Parsing ISO 8601 dates, timezones, etc.

---

### 3.13 Production Server (Alternative to Uvicorn)

#### Gunicorn
```
gunicorn==21.2.0
```
- **Purpose:** Production WSGI server
- **Why:** Stable, well-tested, supports multiple workers
- **When to use:** For WSGI apps (Django), or as alternative to Uvicorn
- **For FastAPI:** Use Uvicorn (ASGI) instead

---

### 3.14 Full Requirements.txt Template

```
# Backend Dependencies
# Core Framework
fastapi==0.103.0
uvicorn[standard]==0.23.2

# Database
sqlalchemy==2.0.20
alembic==1.11.3
psycopg2-binary==2.9.7

# Authentication & Security
pyjwt==2.8.0
bcrypt==4.0.1
python-dotenv==1.0.0

# Caching
redis==5.0.0

# External APIs
requests==2.31.0

# WebSocket
python-socketio==5.9.0
python-engineio==4.7.1

# Validation
pydantic==2.0.3
pydantic-settings==2.0.2

# Background Tasks
apscheduler==3.10.4

# Testing
pytest==7.4.0
pytest-asyncio==0.21.1
pytest-cov==4.1.0

# Utilities
python-dateutil==2.8.2
```

---

## 4. Infrastructure & DevOps

### 4.1 Web Server

#### Nginx
- **Purpose:** Reverse proxy, load balancer
- **Version:** Latest stable (1.24+)
- **Why:** Fast, efficient, handles concurrent connections
- **Configuration:**
  - Proxy requests to Uvicorn backend
  - Serve static frontend files
  - SSL/HTTPS termination
  - Gzip compression

---

### 4.2 Process Manager

#### Supervisor
```
supervisor==4.2.4  # On system, not pip
```
- **Purpose:** Ensure backend process stays running
- **Why:** Auto-restart on crash, logging, simple configuration
- **Alternative:** systemd (systemctl)

---

### 4.3 Containerization (Optional)

#### Docker
- **Purpose:** Container for reproducible deployments
- **Why:** Isolate dependencies, easy deployment
- **Components:**
  - Python image for backend
  - Node image for frontend build
  - PostgreSQL image
  - Redis image
- **docker-compose.yml for local development**

---

### 4.4 Monitoring & Logging

#### System Monitoring
- **Tools:** Basic system monitoring via logs
- **Advanced:** TBD (Prometheus, Grafana for Phase 2+)

---

## 5. Package Installation Commands

### 5.1 Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Or with yarn
yarn install

# Dev server
npm run dev

# Build for production
npm run build

# Run tests
npm test

# Lint code
npm run lint

# Format code
npm run format
```

### 5.2 Backend Setup

```bash
# Navigate to backend directory
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Upgrade pip
pip install --upgrade pip

# Install dependencies
pip install -r requirements.txt

# Run development server
uvicorn app.main:app --reload

# Run tests
pytest

# Generate coverage report
pytest --cov

# Run specific test file
pytest tests/test_auth.py
```

---

## 6. Development Tools (Not in Dependencies)

### 6.1 Version Control
- **Git** - Version control system
- **GitHub** - Code repository hosting

### 6.2 IDEs & Editors
- **VS Code** - Recommended
  - Extensions:
    - Python
    - Pylance
    - ESLint
    - Prettier
    - Thunder Client (API testing)
    - Docker

### 6.3 API Testing
- **Postman** - API testing tool
- **Thunder Client** - VS Code extension
- **curl** - Command-line HTTP client

### 6.4 Database Tools
- **pgAdmin 4** - PostgreSQL management UI
- **DBeaver** - Database client

### 6.5 Browser DevTools
- **Chrome DevTools** - Built-in browser tools
- **Redux DevTools** - Redux state inspection
- **React DevTools** - React component inspection

---

## 7. Package Version Management

### 7.1 Frontend (package.json)
- Use `^` for minor/patch updates (e.g., `^18.2.0` allows 18.x.x)
- Use `~` for patch updates only (e.g., `~18.2.0` allows 18.2.x)
- Pin critical dependencies to exact versions

### 7.2 Backend (requirements.txt)
- Pin exact versions for reproducibility
- Regularly update with `pip install --upgrade -r requirements.txt`
- Review changelogs before major version updates

---

## 8. Security Considerations

### 8.1 Dependency Scanning
- **Frontend:** npm audit, Snyk
- **Backend:** Safety, Bandit
- **Commands:**
  ```bash
  npm audit
  pip install safety && safety check
  ```

### 8.2 Vulnerable Packages
- Subscribe to security advisories
- Regular updates (but test thoroughly)
- Avoid deprecated packages

---

## 9. Performance Optimization

### 9.1 Frontend
- Code splitting with React.lazy()
- Image optimization
- Bundle size monitoring with `webpack-bundle-analyzer`

### 9.2 Backend
- Database connection pooling
- Query optimization
- Response caching with Redis

---

## 10. Alternative Packages (Future Consideration)

### 10.1 GraphQL (Phase 2+)
- **graphene:** Python GraphQL framework
- **apollo-client:** GraphQL client for React

### 10.2 Advanced Caching (Phase 2+)
- **Memcached:** Alternative to Redis
- **Varnish:** HTTP caching

### 10.3 Message Queue (Phase 2+)
- **RabbitMQ:** Message broker
- **Kafka:** Event streaming

### 10.4 Monitoring (Phase 2+)
- **Prometheus:** Metrics collection
- **Grafana:** Metrics visualization
- **ELK Stack:** Logging and analysis

---

## 11. Dependency Lock Files

### 11.1 Frontend
```
package-lock.json (npm) or yarn.lock (yarn)
```
- Ensures reproducible installs
- Commit to version control

### 11.2 Backend
```
requirements.txt (pinned versions)
Pipfile.lock (pipenv alternative)
```
- Ensures reproducible installs
- Commit to version control

---

## 12. Dependency Update Strategy

### 12.1 Regular Updates
- Check for updates monthly
- Test thoroughly after updates
- Update patch versions immediately (security fixes)
- Update minor versions in development
- Update major versions with caution

### 12.2 Security Updates
- Apply immediately after testing
- Security advisories: npm audit, Safety CLI
- Set up Dependabot (GitHub)

---

## 13. Troubleshooting Common Issues

### 13.1 Frontend
- **Conflicting versions:** Delete node_modules and package-lock.json, reinstall
- **Build errors:** Clear .next or build folder, rebuild
- **Port already in use:** Kill process or use different port

### 13.2 Backend
- **Import errors:** Check virtual environment activation
- **Database errors:** Verify PostgreSQL is running
- **Module not found:** Reinstall requirements.txt

---

## 14. Summary Table

| Category | Package | Version | Purpose |
|----------|---------|---------|---------|
| **Frontend Framework** | React | 18.2.0 | UI library |
| **Frontend Build** | Vite | 4.4.9 | Build tool |
| **State Management** | Redux Toolkit | 1.9.5 | State management |
| **HTTP Client** | Axios | 1.4.0 | API calls |
| **WebSocket** | Socket.io-client | 4.7.0 | Real-time updates |
| **Styling** | Tailwind CSS | 3.3.0 | CSS framework |
| **Backend Framework** | FastAPI | 0.103.0 | API server |
| **ASGI Server** | Uvicorn | 0.23.2 | App server |
| **ORM** | SQLAlchemy | 2.0.20 | Database layer |
| **Migrations** | Alembic | 1.11.3 | DB migrations |
| **Database** | PostgreSQL | 12+ | Primary DB |
| **Cache** | Redis | 6+ | Cache store |
| **Auth** | PyJWT | 2.8.0 | JWT tokens |
| **Password** | Bcrypt | 4.0.1 | Password hashing |
| **Testing** | Pytest | 7.4.0 | Test framework |
| **Web Server** | Nginx | 1.24+ | Reverse proxy |

---

## 15. Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-09-12 | Initial technology stack document |

---

**Document Owner:** Technical Lead  
**Last Updated:** 2026-09-12  
**Next Review:** 2026-10-12

