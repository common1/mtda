# Project multi-tenant-document-analysis

## 1. Project Setup & Dependencies

```
[https://www.youtube.com/watch?v=3pZpgNaJe4w&t=428s]
```

```
npx create-next-app@16.4.0 mtda
Need to install the following packages:
create-next-app@16.4.0
Ok to proceed? (y) 

√ Would you like to use the recommended Next.js defaults? » No, customize settings
√ Would you like to use TypeScript? ... Yes
√ Which linter would you like to use? » None
√ Would you like to use React Compiler? ... No
√ Would you like to use Tailwind CSS? ... Yes
√ Would you like your code inside a `src/` directory? ... No
√ Would you like to use App Router? (recommended) ... Yes
√ Would you like to use Cache Components? ... No
√ Would you like to customize the import alias (`@/*` by default)? ... No
√ Would you like to include AGENTS.md to guide coding agents to write up-to-date Next.js code? ... No
√ Would you like to help improve Next.js by letting agents prepare anonymized feedback for your review as you code? (Disable anytime with `experimental.agentFeedback: false`.) ... No
Creating a new Next.js app in D:\Projects_Github\DM_DipeshMalvia\mtda.

Using npm.

Initializing project with template: app-tw 


Installing dependencies:
- next
- react
- react-dom

Installing devDependencies:
- @tailwindcss/turbopack
- @types/node
- @types/react
- @types/react-dom
- tailwindcss
- typescript


added 99 packages, and audited 100 packages in 29s

44 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

Generating route types...
✓ Types generated successfully

Initialized a git repository.

Success! Created mtda at D:\Projects_Github\DM_DipeshMalvia\mtda
```

```
npm run dev
```

## 2. Prisma & Postgres Database Setup

```
[https://www.youtube.com/watch?v=3pZpgNaJe4w&t=653s]
[https://www.prisma.io/docs/guides/v7/frameworks/nextjs]
```

### 2.1 Install and Configure Prisma

```
Install dependencies
npm install prisma@prev tsx @types/pg --save-dev
npm install @prisma/client@7 @prisma/adapter-pg dotenv pg

Initialize prisma
npx prisma init
```

```
Drop database and user

psql -U postgres
postgres=# DROP DATABASE mtda_db; 
postgres=# DROP USER mtda_user; 
```

```
Create database and user

psql -U postgres
postgres=# CREATE DATABASE mtda_db; 
postgres=# CREATE USER mtda_user WITH ENCRYPTED PASSWORD 'WXYZ&6789'; 
postgres=# GRANT ALL PRIVILEGES ON DATABASE mtda_db TO mtda_user; 
postgres=# \c mtda_db postgres;
You are now connected to database "mtda_db" as user "postgres"
mtda_db=# GRANT ALL ON SCHEMA public TO mtda_user; 
mtda_db=# ALTER USER mtda_user CREATEDB;
```

```
Create DATABASE_URL in .env
DATABASE_URL="postgres://mtda_user:WXYZ&6789@localhost:5432/mtda_db"
```

### 2.3 Define your prisma schema

```
[https://www.prisma.io/docs/guides/v7/frameworks/nextjs#22-define-your-prisma-schema]
```

```
In prisma/schema.prisma add the following models

model User { 
  id    Int     @id @default(autoincrement()) 
  email String  @unique
  name  String?
} 
 ```

### 2.4 Run migrations and generate Prisma Client

```
npx prisma migrate dev --name init
```

```
Generate prisma client
npx prisma generate
```

```
Start prisma studio

npx prisma studio
```

```
Remove all items from the database

npx prisma migrate reset
```

```
Setup Prisma Client
File: /lib/prisma.ts
import { PrismaClient } from "../app/generated/prisma/client"; 
import { PrismaPg } from "@prisma/adapter-pg"; 
const globalForPrisma = global as unknown as {
  prisma: PrismaClient; 
}; 
const adapter = new PrismaPg({
  connectionString: process.env.DATABASE_URL, 
}); 
const prisma =
  globalForPrisma.prisma ||
  new PrismaClient({
    adapter, 
  }); 
if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma; 
export default prisma;
```

### 3. Setup Shadcn/UI

```
npx shadcn@4.22.1 init
npx shadcn@4.22.1 init
Need to install the following packages:
shadcn@4.22.1
Ok to proceed? (y) 

√ Select a component library » Base UI (Recommended)
√ Which preset would you like to use? » Nova
✔ Preflight checks.
✔ Verifying framework. Found Next.js.
✔ Validating Tailwind CSS. Found v4.
✔ Validating import alias.
✔ Writing components.json.
✔ Checking registry.
✔ Installing dependencies.
✔ Updating fonts.
✔ Created 2 files:
  - components\ui\button.tsx
  - lib\utils.ts
✔ Updating app\globals.css

Project initialization completed.
You may now add components.
```


