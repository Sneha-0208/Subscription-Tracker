# Subscription Tracker API

A simple REST API for tracking subscription services, managing users, and automating subscription workflows.

## Introduction

The Subscription Tracker API provides backend support to store, retrieve, and manage recurring subscriptions. It includes user authentication, subscription CRUD operations, and workflow routes for scheduling or automating subscription-related tasks.

## Features

- User registration and login
- Create, update, delete, and list subscriptions
- Subscription tracking with status and frequency
- Workflow endpoints for managing scheduled subscription tasks
- Error handling middleware for clean API responses
- Rate limiting and protection against bots (using Arcjet)

## Tools Used

- Node.js
- Express.js
- MongoDB
- Mongoose
- Nodemailer (send reminder emails)
- Arcjet (custom middleware/config)
- Upstash (schedule reminder workflows)
- Thuder Client

## Local Setup

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd subscription-tracker
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Store environment variables in `.env` file
4. Configure environment variables and application settings in the `config` folder.
   - `config/env.js`
   - `config/arcjet.js`
   - `config/nodemailer.js`
   - `config/upstash.js`
5. Make sure MongoDB is running locally or update the connection string in `database/mongodb.js`.

## How to Run

Start the app with:

```bash
npm run dev
```

Then open the API at:

```text
http://localhost:5500
```
Open another terminal to run the workflows by using the command:
```
npx @upstash/qstash-cli dev
```

## Project Structure

- `app.js` — app entry point
- `config/` — configuration files
- `controllers/` — request handlers
- `database/` — MongoDB connection
- `middlewares/` — custom middleware
- `models/` — Mongoose schemas
- `routes/` — API route definitions
- `utils/` — helper utilities
- `.env.<development/production>.local` — store environment variables

## Notes

Update any configuration files or environment values needed for email delivery, database access, and scheduled workflows before running the API.

## Future Improvements
- Implement api/v1/auth/sign-out route 
- Complete the API endpoints implementation in subscription.routes.js
- Design an appealing UI