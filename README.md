# HRMS API

An internship/learning project implementing a Human Resource Management System (HRMS) backend with **NestJS**, **Prisma**, and JWT-based authentication with OTP verification. Companion frontend: [hrms-app](../hrms-app).

## Features

- **Auth** — JWT login with OTP email verification (Postmark), Passport JWT/local strategies
- **User & Administrator** — separate account types with role-based access
- **Role** — role definitions/assignment
- **Employee** — employee records management
- **Attendance** — check-in/check-out and attendance record tracking
- **Leave** — leave requests, comments on leave requests, and mentioned-employee notifications
- **Background jobs** — Redis-backed queue via `@nestjs/bull` (e.g. for async email/notification processing)
- **Real-time** — WebSocket support via `@nestjs/websockets` + `socket.io`
- **Exports** — CSV export support (`csv-writer`, `json2csv`) and PDF/report generation via `puppeteer`
- **Swagger** — interactive API docs generated from decorators
- **Deployable** — configured for both Docker (`Dockerfile`, `docker-compose.yml`) and Vercel serverless (`vercel.json`, `api/index.ts`, AWS Lambda adapter)

## Tech Stack

- [NestJS 10](https://nestjs.com/) (TypeScript)
- [Prisma](https://www.prisma.io/) ORM
- Passport + `@nestjs/jwt` for authentication
- Redis + Bull for job queues
- Socket.IO for real-time events
- Postmark for transactional email
- Docker & Vercel serverless deployment targets
- Jest for unit/e2e testing

## Data Model

Defined in `prisma/schema.prisma`:

- `User`, `UserCreadentials`, `UserSession` — accounts & sessions
- `Role` — role-based permissions
- `Employee` — employee records
- `AttendanceRecord` — attendance/check-in data
- `LeaveRequest`, `CommentsOnLeave`, `MentionedEmployees` — leave management workflow

## Project Structure

```
src/
├── auth/            # JWT auth, OTP verification, Passport strategies
├── user/            # user accounts
├── administrator/   # admin accounts
├── role/            # role management
├── employee/        # employee CRUD
├── attendance/       # attendance/check-in/out
├── leave/           # leave requests & comments
├── shared/          # shared keys/methods/email templates
├── validator/       # custom DTO validators
└── prisma/          # Prisma service/module
api/
└── index.ts         # Vercel serverless entry point
```

## Getting Started

### Prerequisites
- Node.js 18+
- A database reachable via `DATABASE_URL` (Prisma)
- Redis (for the Bull job queue)
- A Postmark API key (OTP/notification email)

### Setup

```bash
git clone https://github.com/vsmm-world/hrms-api.git
cd hrms-api
npm install
```

Create a `.env` file in the project root:

```bash
DATABASE_URL="<your-database-connection-string>"
PORT=3000
REDIS_HOST="127.0.0.1"
REDIS_PORT=6379
POST_MARK_API_KEY="your-postmark-api-key"
FROM_MAIL="no-reply@example.com"
ADMIN_EMAIL="admin@example.com"
```

Generate the Prisma client and sync the schema:

```bash
npm run prisma-all
```

Run the app:

```bash
npm run start:dev
```

### Tests

```bash
npm run test        # unit tests
npm run test:e2e     # e2e tests
npm run test:cov     # coverage report
```

### Deployment

- **Docker**: `docker-compose up --build`
- **Vercel**: `npm run vercel-build` (uses `api/index.ts` as the serverless entry point)

## Author

**Ravindra Valand**
- GitHub: [@vsmm-world](https://github.com/vsmm-world)
- Instagram: [@ravindra_valand](https://www.instagram.com/ravindra_valand/)

## License

UNLICENSED — internship/learning project, not intended for production use.
