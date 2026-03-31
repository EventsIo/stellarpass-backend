# StellarPass Backend

NestJS REST API server for the StellarPass event ticketing dApp. Handles organizer authentication, event management, invite code generation, IPFS metadata pinning, Stellar Horizon webhooks, and analytics.

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | NestJS + TypeScript |
| Database | PostgreSQL + TypeORM |
| Auth | JWT + bcrypt |
| Blockchain | `@stellar/stellar-sdk` |
| File Storage | IPFS via Pinata |
| QR Codes | `qrcode` |
| Validation | `class-validator` + `class-transformer` |

## Prerequisites

- Node.js >= 20
- PostgreSQL >= 15
- A [Pinata](https://pinata.cloud) account for IPFS pinning
- A Stellar testnet account

## Project Structure

```
stellarpass-backend/
├── src/
│   ├── auth/               # JWT auth, login, register, password reset
│   ├── users/              # Organizer user entity and service
│   ├── events/             # Event CRUD, status management
│   ├── tickets/            # Ticket records, QR code generation
│   ├── invites/            # Invite code generation and validation
│   ├── payments/           # Payment records, refund logic
│   ├── analytics/          # Dashboard stats, CSV export
│   ├── stellar/            # Stellar SDK service, Horizon webhooks
│   ├── ipfs/               # Pinata IPFS upload service
│   ├── common/
│   │   ├── decorators/     # Custom decorators (e.g. @CurrentUser)
│   │   ├── guards/         # Auth guards, plan guards
│   │   ├── filters/        # Global exception filters
│   │   ├── interceptors/   # Logging, response transform
│   │   └── pipes/          # Validation pipes
│   ├── config/             # App config (env vars)
│   ├── database/
│   │   └── migrations/     # TypeORM migrations
│   └── main.ts             # App entry point
├── test/                   # e2e tests
├── docs/
│   └── api.md              # API endpoint documentation
├── .github/
│   └── workflows/
│       └── ci.yml
├── .env.example
├── package.json
└── README.md
```

## Getting Started

```bash
# Clone the repo
git clone https://github.com/your-org/stellarpass-backend.git
cd stellarpass-backend

# Install dependencies
npm install

# Copy env file and fill in values
cp .env.example .env

# Run database migrations
npm run migration:run

# Start development server
npm run start:dev
```

## API Modules

| Module | Base Route | Description |
|---|---|---|
| Auth | `/auth` | Register, login, password reset |
| Users | `/users` | Organizer profile, plan management |
| Events | `/events` | Create, update, cancel, list events |
| Tickets | `/tickets` | Ticket records, QR generation, check-in |
| Invites | `/invites` | Generate and validate invite codes |
| Payments | `/payments` | Payment records, refund processing |
| Analytics | `/analytics` | Dashboard stats, CSV export |

## Available Scripts

```bash
npm run start:dev       # Start in watch mode
npm run build           # Build for production
npm run start:prod      # Start production build
npm run test            # Run unit tests
npm run test:e2e        # Run e2e tests
npm run migration:run   # Run pending migrations
npm run migration:revert # Revert last migration
npm run migration:generate -- src/database/migrations/MigrationName  # Generate migration
```

## Environment Variables

See `.env.example` for all required variables.

## Contributing

See [docs/CONTRIBUTING.md](./docs/CONTRIBUTING.md) for guidelines.

## License

MIT
