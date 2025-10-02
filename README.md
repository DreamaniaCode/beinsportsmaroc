# beinsportsmaroc

A modern sports streaming platform inspired by beIN SPORTS, built for the Moroccan market with full Arabic, French, and English language support.

## Project Overview

beinsportsmaroc.com is a comprehensive sports platform offering:

- **Live Streaming**: Real-time sports event broadcasts
- **Live Scores**: Goal-by-goal updates with WebSocket integration
- **Sports News**: Latest articles covering football, basketball, tennis, and more
- **TV Guide**: Electronic program guide with channel schedules
- **Multi-language Support**: Arabic (default), French, and English
- **User Profiles**: Personalized experience with favorite teams and leagues
- **Catch-up**: Watch events that have already started
- **Highlights & Replays**: Video on demand content

## Technology Stack

### Frontend
- **Framework**: Next.js 14 (React 18)
- **Styling**: CSS with RTL support
- **Internationalization**: i18n with Arabic as default
- **Real-time**: Socket.io client

### Backend
- **Framework**: NestJS (Node.js)
- **Database**: PostgreSQL
- **ORM**: Prisma
- **Authentication**: JWT + Passport
- **Real-time**: Socket.io + WebSocket Gateway

### Infrastructure
- **Containerization**: Docker + Docker Compose
- **Reverse Proxy**: Nginx
- **CI/CD**: GitHub Actions (planned)

## Project Structure

```
beinsportsmaroc/
├── docs/                    # Documentation
│   ├── README-AR.md        # Arabic documentation
│   ├── api-specs.md        # API specifications
│   └── architecture.md     # Architecture diagrams
├── infra/                  # Infrastructure configuration
│   ├── docker/
│   │   ├── docker-compose.yml
│   │   ├── nginx.conf
│   │   └── .env.example
│   └── ci-cd/
├── backend/                # NestJS backend
│   ├── src/
│   │   ├── modules/
│   │   │   ├── auth/      # Authentication
│   │   │   ├── scores/    # Live scores
│   │   │   ├── news/      # News articles
│   │   │   ├── tvguide/   # TV scheduling
│   │   │   └── users/     # User management
│   │   ├── database/      # Prisma schema & migrations
│   │   └── websockets/    # Real-time updates
│   └── package.json
└── frontend/               # Next.js frontend
    ├── src/
    │   ├── pages/         # Next.js pages
    │   ├── components/    # React components
    │   ├── styles/        # Global styles with RTL
    │   └── i18n/          # Translations (ar, fr, en)
    └── package.json
```

## Getting Started

### Prerequisites
- Node.js 18+ LTS
- Docker & Docker Compose (recommended)
- PostgreSQL 15+ (if not using Docker)

### Installation with Docker (Recommended)

1. Clone the repository:
```bash
git clone https://github.com/DreamaniaCode/beinsportsmaroc.git
cd beinsportsmaroc
```

2. Configure environment:
```bash
cd infra/docker
cp .env.example .env
# Edit .env with your configuration
```

3. Start all services:
```bash
docker-compose up -d
```

This will start:
- PostgreSQL database (port 5432)
- Backend API (port 4000)
- Frontend (port 3000)
- Nginx reverse proxy (port 80)

4. Access the application:
- Frontend: http://localhost:3000
- Backend API: http://localhost:4000
- Full site via Nginx: http://localhost

### Manual Installation

#### Backend Setup
```bash
cd backend
cp .env.example .env
# Configure DATABASE_URL and JWT_SECRET in .env
npm install
npm run db:push
npm run start:dev
```

#### Frontend Setup
```bash
cd frontend
cp .env.local.example .env.local
# Configure NEXT_PUBLIC_API_BASE in .env.local
npm install
npm run dev
```

## Features Roadmap

### Phase 1: Core Platform ✅
- [x] Project structure setup
- [x] Basic frontend with Arabic RTL support
- [x] Backend API with NestJS
- [x] Live scores module
- [x] News articles module
- [x] Docker containerization

### Phase 2: Live Features (In Progress)
- [ ] WebSocket real-time score updates
- [ ] Live streaming integration
- [ ] TV guide with EPG data
- [ ] User authentication & profiles

### Phase 3: Enhanced Features
- [ ] Sports data API integration (Sportradar/Opta)
- [ ] Video streaming infrastructure
- [ ] Push notifications
- [ ] Mobile responsive optimization
- [ ] SEO optimization

### Phase 4: Monetization
- [ ] Subscription management (Stripe)
- [ ] Premium content access control
- [ ] Advertisement integration
- [ ] Analytics dashboard

## API Endpoints

### Scores
- `GET /api/scores` - Get live scores
- `WS /ws/scores` - WebSocket for real-time updates

### News
- `GET /api/news` - List news articles
- `GET /api/news/:id` - Get single article

### TV Guide
- `GET /api/tv` - Get TV schedule

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout

## Development

### Database Migrations
```bash
cd backend
npm run db:migrate
```

### Code Style
- Backend: ESLint + Prettier
- Frontend: ESLint + Prettier

### Testing
```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test
```

## Deployment

### Production Build
```bash
# Backend
cd backend
npm run build
npm run start:prod

# Frontend
cd frontend
npm run build
npm start
```

### Environment Variables

#### Backend (.env)
```env
DATABASE_URL=postgresql://user:password@host:5432/dbname
JWT_SECRET=your-secret-key
PORT=4000
```

#### Frontend (.env.local)
```env
NEXT_PUBLIC_API_BASE=http://localhost:4000
NEXT_PUBLIC_WS_URL=ws://localhost:4000
```

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contact

Project Link: [https://github.com/DreamaniaCode/beinsportsmaroc](https://github.com/DreamaniaCode/beinsportsmaroc)

## Acknowledgments

- Inspired by beIN SPORTS platform
- Built with modern web technologies
- Designed for the Moroccan sports community

---

**Status**: 🚧 Under Active Development

**Version**: 1.0.0

**Last Updated**: October 2025
