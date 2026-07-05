# Subscription Tracker API

A RESTful backend application for managing recurring subscriptions, user authentication, and automated renewal reminders. The API allows users to manage their subscriptions while automatically scheduling reminder emails before each renewal date.

## Features

- JWT-based user authentication and authorization
- User registration and login
- Create, update, delete, and manage subscriptions
- Automatic renewal date calculation
- Email reminders scheduled before subscription renewal
- Persistent workflow scheduling using Upstash Workflow
- Rate limiting and bot protection using Arcjet
- Centralized error handling middleware
- MongoDB database integration using Mongoose

## Tech Stack

| Technology | Purpose |
|------------|---------|
| Express.js | Backend REST API |
| MongoDB + Mongoose | Database & ODM |
| JWT | Authentication & Authorization |
| Arcjet | Rate Limiting & Bot Protection |
| Upstash Workflow | Long-running workflow scheduling |
| Nodemailer | Email service |
| Gmail SMTP | Email delivery |
| Thunder Client | API Testing |

# Architecture

## Overall Architecture

```text
                         User
                           │
                           ▼
                  Express.js API Server
                           │
          ┌────────────────┴────────────────┐
          │                                 │
          ▼                                 ▼
     Arcjet Security                JWT Authentication
 (Rate Limiting & Bot Detection)          │
          │                               ▼
          │                         Authorization
          │                               │
          └──────────────┬────────────────┘
                         ▼
                   Route Handlers
                         │
                         ▼
                    Controllers
                         │
         ┌───────────────┼────────────────┐
         │               │                │
         ▼               ▼                ▼
     MongoDB         Upstash Workflow   Nodemailer
 (Users & Subs)    (Schedule Emails)  (Send Emails)
         │               │                │
         │               │                ▼
         │               │           Gmail SMTP
         │               │                │
         │               └────────────────┘
         │                        │
         └────────────────────────▼
                           User Inbox
```

## Request Flow

```text
User
  │
  ▼
HTTP Request
  │
  ▼
Arcjet Middleware
  │
  ▼
JWT Authorization Middleware
  │
  ▼
Express Route
  │
  ▼
Controller
  │
  ▼
MongoDB
  │
  ├──────────────► Return Data
  │
  └──────────────► Trigger Upstash Workflow
                          │
                          ▼
                  Wait Until Reminder Date
                          │
                          ▼
                    Nodemailer
                          │
                          ▼
                     Gmail SMTP
                          │
                          ▼
                    User Receives Email
```

## Component Responsibilities

### Express.js
- Handles incoming HTTP requests.
- Routes requests to the appropriate controllers.

### Arcjet
- Protects APIs from bots.
- Applies rate limiting.
- Prevents malicious requests before they reach the application.

### JWT Authentication
- Authenticates users using JSON Web Tokens.
- Stores authenticated user information in `req.user`.

### Controllers
Implements the application's business logic:
- Authentication
- User Management
- Subscription Management
- Reminder Scheduling

### MongoDB
Stores:
- User information
- Subscription details

### Upstash Workflow
Schedules reminder emails:
- 7 days before renewal
- 3 days before renewal
- 1 day before renewal

Since workflows are managed externally, reminders continue working even if the backend restarts.

### Nodemailer
Sends reminder emails through Gmail SMTP.

### Gmail SMTP
Delivers emails to the user's inbox.

# Project Structure

```text
subscription-tracker/
│── config/
│── controllers/
│── database/
│── middlewares/
│── models/
│── routes/
│── utils/
│── app.js
│── package.json
```

# Local Setup

## Prerequisites

- Node.js (v18+ recommended)
- MongoDB
- Gmail App Password
- Upstash Workflow account
- Arcjet account

## Installation

```bash
git clone https://github.com/Sneha-0208/Subscription-Tracker.git
cd subscription-tracker
npm install
```

## Environment Variables

Create a `.env` file in the project root and configure the required environment variables.

Example:

```env
PORT=

MONGO_URI=

JWT_SECRET=
JWT_EXPIRES_IN=

EMAIL_USER=
EMAIL_PASSWORD=

ARCJET_KEY=

QSTASH_TOKEN=
QSTASH_URL=
```

## Running the Application

Start the backend server:

```bash
npm run dev
```

The server will be available at:

```text
http://localhost:5500
```

Start the Upstash Workflow development server in a separate terminal:

```bash
npx @upstash/qstash-cli dev
```

# Future Improvements

- Implement the `api/v1/auth/sign-out` endpoint.
- Complete the remaining Subscription CRUD endpoints.
- Add API documentation using Swagger/OpenAPI.
- Containerize the application using Docker.
- Deploy the backend to a cloud platform.
- Build a frontend dashboard for subscription management.


# Notes

- Configure MongoDB, Gmail SMTP, Arcjet, and Upstash credentials before running the application.
- Email reminders are scheduled using Upstash Workflow and continue to execute independently of the backend server.
- Gmail SMTP requires an **App Password**, not your regular Gmail password.