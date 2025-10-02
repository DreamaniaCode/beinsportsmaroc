# beinsportsmaroc - Project Setup Guide

This guide provides step-by-step instructions to set up the complete project structure and files.

## Initial Setup

The repository has been initialized with:
- ✅ README.md with full project documentation
- ✅ Docker Compose configuration
- ✅ Environment variable template
- ✅ Backend package.json with NestJS dependencies

## Complete Project Structure Setup

### Method 1: Using Git (Recommended)

Clone the repository and create remaining files:

```bash
git clone https://github.com/DreamaniaCode/beinsportsmaroc.git
cd beinsportsmaroc
```

### Create Frontend Package.json

Create `frontend/package.json`:

```json
{
  "name": "beinsportsmaroc-frontend",
  "version": "1.0.0",
  "description": "Frontend for beinsportsmaroc - Next.js application",
  "private": true,
  "scripts": {
    "dev": "next dev -p 3000",
    "build": "next build",
    "start": "next start -p 3000",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "14.2.5",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "socket.io-client": "^4.6.0"
  },
  "devDependencies": {
    "@types/node": "^20.11.0",
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "eslint": "^8.42.0",
    "eslint-config-next": "14.2.5",
    "typescript": "^5.4.0"
  }
}
```

### Create Nginx Configuration

Create `infra/docker/nginx.conf`:

```nginx
events {
    worker_connections 1024;
}

http {
    upstream backend {
        server backend:4000;
    }

    upstream frontend {
        server frontend:3000;
    }

    server {
        listen 80;
        server_name _;

        location /api/ {
            proxy_pass http://backend/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }

        location /ws/ {
            proxy_pass http://backend/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
        }

        location / {
            proxy_pass http://frontend/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
}
```

### Create Backend Dockerfile

Create `backend/Dockerfile`:

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

EXPOSE 4000

CMD ["npm", "run", "start:prod"]
```

### Create Frontend Dockerfile

Create `frontend/Dockerfile`:

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

### Create Backend Source Files

Create the following backend files:

#### `backend/src/main.ts`:
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import * as cookieParser from 'cookie-parser';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.enableCors({ origin: process.env.CORS_ORIGIN || 'http://localhost:3000', credentials: true });
  app.use(cookieParser());
  app.setGlobalPrefix(process.env.API_PREFIX || 'api');
  await app.listen(process.env.PORT || 4000);
  console.log(`Backend running on port ${process.env.PORT || 4000}`);
}
bootstrap();
```

#### `backend/src/app.module.ts`:
```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
    }),
  ],
})
export class AppModule {}
```

#### `backend/tsconfig.json`:
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "ES2021",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": false,
    "noImplicitAny": false,
    "strictBindCallApply": false,
    "forceConsistentCasingInFileNames": false,
    "noFallthroughCasesInSwitch": false
  }
}
```

### Create Frontend Source Files

#### `frontend/next.config.js`:
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  i18n: {
    locales: ['ar', 'fr', 'en'],
    defaultLocale: 'ar',
  },
}

module.exports = nextConfig
```

#### `frontend/tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

#### `frontend/src/pages/_app.tsx`:
```typescript
import type { AppProps } from 'next/app';
import { useEffect } from 'react';
import '../styles/globals.css';

export default function App({ Component, pageProps }: AppProps) {
  useEffect(() => {
    document.documentElement.setAttribute('dir', 'rtl');
    document.documentElement.setAttribute('lang', 'ar');
  }, []);

  return <Component {...pageProps} />;
}
```

#### `frontend/src/pages/index.tsx`:
```typescript
import Head from 'next/head';

export default function Home() {
  return (
    <>
      <Head>
        <title>beinsportsmaroc.com - بين سبورتس المغرب</title>
        <meta name="description" content="Sports streaming platform for Morocco" />
      </Head>
      <main>
        <div className="banner">
          <h1>beinsportsmaroc.com</h1>
          <p>بث مباشر، نتائج حية، وآخر الأخبار الرياضية</p>
        </div>
      </main>
    </>
  );
}
```

#### `frontend/src/styles/globals.css`:
```css
:root {
  --primary: #6b2fb3;
  --dark: #0f0f20;
  --text: #fff;
}

html[dir="rtl"] {
  direction: rtl;
}

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-family: 'Tajawal', system-ui, -apple-system, Arial;
  background: var(--dark);
  color: var(--text);
}

.banner {
  background: linear-gradient(120deg, #6b2fb3, #2f6bb3);
  padding: 4rem 2rem;
  text-align: center;
}

.banner h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
}
```

## Quick Start After Setup

### Using Docker:
```bash
cd infra/docker
cp .env.example .env
docker-compose up -d
```

### Manual Setup:
```bash
# Backend
cd backend
npm install
npm run start:dev

# Frontend (in new terminal)
cd frontend
npm install
npm run dev
```

## Next Steps

1. **Database Setup**: Configure PostgreSQL and run migrations
2. **API Development**: Implement modules for scores, news, auth
3. **Frontend Components**: Build React components
4. **Testing**: Set up Jest and write tests
5. **Deployment**: Configure production environment

## Support

For issues or questions, open an issue on GitHub.

---

**Repository**: https://github.com/DreamaniaCode/beinsportsmaroc
**Status**: 🚀 Ready for Development
