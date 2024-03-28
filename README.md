# Prisma Pulse starter project

This repository has been created to help you get started with [Pulse](https://prisma.io/pulse). You will be able to use this project with any Pulse-ready PostgreSQL database. This project comes with a basic [`prisma.schema`](./prisma/schema.prisma) file as well as a Pulse subscription found in the [`index.ts`](./index.ts) file.

## Table of contents

- [Prerequisites](#prerequisites)
- [Local development](#local-development)
  - [1. Clone the respository](#1-clone-the-respository)
  - [2. Create and fill out a `.env` file](#2-create-and-fill-out-a-env-file)
  - [3. Run the database migration](#3-run-the-database-migration)
  - [4. Start the Pulse subscription](#4-start-the-pulse-subscription)
  - [5. Test the subscription](#5-test-the-subscription)
- [Deployment](#deployment)
  - [Deploy the Pulse Starter Project on railway.app](#deploy-the-pulse-starter-project-on-railwayapp)
- [More example Pulse projects](#more-example-pulse-projects)
- [More information about Pulse](#more-information-about-pulse)

## Prerequisites

To successfully run the project, you will need the following:

- A Prisma Pulse-compatible database
  - Refer to our comprehensive guide [here](https://www.prisma.io/docs/pulse/database-setup) for setup instructions, or deploy a Pulse-ready database on Railway [![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/pulse-pg?referralCode=VQ09uv)
- An active [Prisma Data Platform](https://pris.ly/pdp) account for obtaining a Prisma Pulse API Key
  
## Local development

### 1. Clone the respository

Clone the repository, navigate into it and install dependencies:

```bash
git clone https://github.com/prisma/pulse-starter.git
cd pulse-starter
npm install
```

### 2. Create and fill out a `.env` file

Rename the existing `.env.example` to `.env`:

```bash
mv .env.example .env
```

<details><summary>The <code>.env</code> file contains the following environment variables:</summary>

    DATABASE_URL=""
    PULSE_API_KEY=""

</details>

- `DATABASE_URL`: The connection string to your database.
- `PULSE_API_KEY`: Reference the [Environment API Keys](https://www.prisma.io/docs/platform/concepts/environments#api-keys) section in our documentation to learn how get an API key for your Pulse project.

### 3. Run the database migration

The `prisma/schema.prisma` contains three models based on our [hello-prisma](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/using-prisma-migrate-typescript-postgresql) example project:

```bash
npx prisma migrate dev --name init
```

### 4. Start the Pulse subscription

Run the [script](./index.ts) that contains the code to subscribe to database events:

```bash
npx ts-node index.ts
```

This will run a basic subscription on the `User` table. The code can be found in the [`index.ts`](./index.ts) file. To learn more about the Pulse API and how to use it, check out our [documentation](https://www.prisma.io/docs/data-platform/pulse/api-reference#subscribe).

<details><summary>Pulse user table subscription</summary>

```ts
async function main() {
 const subscription = await prisma.user.subscribe();

 if (subscription instanceof Error) {
  throw subscription;
 }

 for await (const event of subscription) {
  console.log("just received an event:", event);
 }
}
```

</details>

### 5. Test the subscription

The following instructions uses [Prisma Studio](https://www.prisma.io/studio) to create a new record in the `User` table. However, you can use any other method to write to the `User` table (e.g. a SQL client like `psql` or [TablePlus](https://tableplus.com/)) in order to trigger a database change event in Pulse.

1. Start Prisma Studio in a new terminal: `npx prisma studio`
2. Add a new record to the `User` table from Prisma Studio.
3. Return to your terminal where you ran the `npx ts-node index.ts` command.
4. If everything is set up properly you will see an output that is similar to the following.

```json
{
 "action": "create",
 "created": {
  "id": 1,
  "email": "test@prisma.io",
  "name": "test"
 }
}
```

## Deployment

### Deploy the Pulse Starter Project on railway.app
You can also deploy this project on Railway by following the instructions in our [docs](https://www.prisma.io/docs/pulse/database-setup/railway#setup-with-template-prisma-pulse-db--app).
[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/pulse-starter?referralCode=VQ09uv)

This project is used together with the [prisma/pulse-railway-pg-config](https://github.com/prisma/pulse-railway-pg-config) repo to build a template on [railway.app](https://railway.app).

[![Watch the setup template setup video](https://img.youtube.com/vi/0nt7CLDqYeY/0.jpg)](https://www.youtube.com/watch?v=0nt7CLDqYeY)

>  For the most current configuration details, please refer to our [documentation](https://www.prisma.io/docs/pulse/database-setup/railway), as information in the video may be outdated.

## More example Pulse projects

- [Prisma Pulse examples](https://pris.ly/pulse-examples)

## More information about Pulse

- [Pulse documentation](https://pris.ly/pulse-docs)
- [Pulse GA announcement blog post](https://pris.ly/gh/pulse-ga)
