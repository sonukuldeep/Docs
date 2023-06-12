## Prisma

### 1. Installation

```js
npm i -D prisma
npm i @prisma/client
```

### 2. Initialize prisma

```js
npx prisma init // this creates a prisma/schema.prisma file where you put your schema
// npx prisma init --datasource-provider postgresql
// npx prisma init --datasource-provider sqlite
```

Example:-

```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String?
  posts Post[]
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
}
```

### 3. Run migration to create your database

#### 1. Prisma Migrate in development environments

Make sure you have added schema in the above stems before running this command <br>
In a development environment, use the migrate dev command to generate and apply migrations:

```js
npx prisma migrate dev --name init
```

> Note: migrate dev is a development command and should never be used in a production environment.

##### Reset the development database

```js
npx prisma migrate reset
```

#### 2. Prisma Migrate in production and testing environments

In production and testing environments, use the migrate deploy command to apply migrations:

```js
npx prisma migrate deploy
```

> Note: migrate deploy should generally be part of an automated CI/CD pipeline, and we do not recommend running this command locally to deploy changes to a production database.

[read more](https://www.prisma.io/docs/concepts/components/prisma-migrate/migrate-development-production#production-and-testing-environments)

<hr>

### Prisma Studio

```js
npx prisma studio // GUI to view and edit data in your database
```

<hr>

### Prisma best practice

In development, the command next dev clears Node.js cache on run. This in turn initializes a new PrismaClient instance each time due to hot reloading that creates a connection to the database. This can quickly exhaust the database connections as each PrismaClient instance holds its own connection pool.<br>

The solution in this case is to instantiate a single instance PrismaClient and save it on the global object. Then we keep a check to only instantiate PrismaClient if it's not on the global object otherwise use the same instance again if already present to prevent instantiating extra PrismaClient instances.<br>
Create a folder lib/prisma.ts in root <br>
lib/prisma.ts

```ts
import { PrismaClient } from "@prisma/client";

// PrismaClient is attached to the `global` object in development to prevent
// exhausting your database connection limit.
//
// Learn more:
// https://pris.ly/d/help/next-js-best-practices

const globalForPrisma = global as unknown as { prisma: PrismaClient };

export const prisma = globalForPrisma.prisma || new PrismaClient();

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma;

export default prisma;
```

<hr>

### Prisma in vercel

Resolve dependency caching error
[link](https://www.prisma.io/docs/guides/other/troubleshooting-orm/help-articles/vercel-caching-issue) <br>
Use the [custom postinstall script](https://www.prisma.io/docs/guides/other/troubleshooting-orm/help-articles/vercel-caching-issue#a-custom-postinstall-script) method

### Prisma in Netlify

Resolve dependency caching error
[link](https://www.prisma.io/docs/guides/other/troubleshooting-orm/help-articles/netlify-caching-issue) <br>
Use the [custom postinstall script](https://www.prisma.io/docs/guides/other/troubleshooting-orm/help-articles/netlify-caching-issue#a-custom-postinstall-script) method
