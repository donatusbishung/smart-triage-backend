# Smart Triage Backend

A Node.js and Express backend for a smart support-ticket triage system. It lets customers submit tickets, automatically classifies them with Gemini AI, and provides authenticated support-agent endpoints to review and update tickets.

## Features

- User registration and login with JWT authentication
- Public ticket creation endpoint for customer submissions
- AI-assisted ticket categorization and priority assignment
- Authenticated ticket listing, detail retrieval, and status updates
- MongoDB-backed persistence with Mongoose

## Tech Stack

- Node.js
- Express
- MongoDB + Mongoose
- JSON Web Tokens (JWT)
- Google Gemini AI
- Jest + Supertest for tests

## Prerequisites

Before running this project, make sure you have:

- Node.js (18 or newer recommended)
- npm
- MongoDB running locally or a reachable MongoDB URI
- A Gemini API key from Google AI Studio

## Environment Variables

Create a .env file in the project root with the following variables:

```env
PORT=3001
MONGO_URI=mongodb://localhost:27017/mydb
JWT_SECRET=your_jwt_secret
GEMINI_API_KEY=your_gemini_api_key
```

## Installation

```bash
npm install
```

## Running the Server

Start the app in development mode:

```bash
npm run dev
```

Start the production server:

```bash
npm start
```

The API will run on the port defined in PORT (default: 3001).

## API Overview

### Authentication

- POST /api/auth/register
  - Register a support agent
  - Body: name, email, password

- POST /api/auth/login
  - Authenticate a support agent
  - Body: email, password

### Tickets

- POST /api/tickets
  - Create a new support ticket
  - Public endpoint
  - Body: name, email, subject, description

- GET /api/tickets
  - List tickets
  - Requires JWT bearer token
  - Supports query parameters: page, limit, status, priority

- GET /api/tickets/:id
  - Get one ticket by ID
  - Requires JWT bearer token

- PATCH /api/tickets/:id
  - Update ticket status
  - Requires JWT bearer token
  - Body: status

## Example Usage

### Register an agent

```bash
curl -X POST http://localhost:3001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Support Agent",
    "email": "agent@example.com",
    "password": "secret123"
  }'
```

### Create a ticket

```bash
curl -X POST http://localhost:3001/api/tickets \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Customer Name",
    "email": "customer@example.com",
    "subject": "Login issue",
    "description": "I cannot reset my password from the dashboard."
  }'
```

### List tickets with auth

```bash
curl -X GET http://localhost:3001/api/tickets \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## Testing

Run the test suite:

```bash
npm test
```

The current test setup uses Jest with coverage reporting.

## Project Notes

- The ticket creation flow uses AI to infer the category and priority when the request is submitted.
- If AI analysis fails, the service falls back to a safe default category and priority.
- MongoDB connection details are controlled via the MONGO_URI environment variable.
