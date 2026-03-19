# Quiz

[▶️ Смотреть демо](demo/demo.mp4)

A lightweight quiz application with a Node.js/Express backend and a plain-JavaScript frontend built with Webpack. The
backend uses JSON files to store users, tests and results.

---

## Table of Contents

- Project overview
- Features
- Technology stack
- Repository layout
- Getting started
    - Prerequisites
    - Install
    - Running backend
    - Running frontend (development)
    - Building frontend for production
- Configuration
- API (summary)
    - Authentication endpoints
    - Test endpoints
- Data & persistence

---

## Project overview

Quiz is a demo quiz platform. Users can:

- Sign up / log in (JWT-based flow).
- Choose tests/topics.
- Start a quiz, navigate questions, submit answers.
- See results and correct answers.

The server exposes a simple REST API consumed by the frontend. Data is stored in JSON files and accessed via TAFFY.

## Features

- Authentication: sign up, login, token refresh, logout.
- Fetching available tests and test details.
- Taking tests and submitting answers.
- Viewing results and viewing correct answers for a finished test.
- Simple frontend with templates, styles and modular JS components.

## Technology stack

- Backend: Node.js, Express, TAFFY (JSON-backed), JWT for tokens.
- Frontend: Plain ES modules, Webpack, HTML/CSS templates.
- Tools: webpack-dev-server (frontend dev), node.js (backend)

## Repository layout

```
backend/
  app.js                     Express entry point (port 3000)
  routes/                    Route definitions (auth, tests)
  controllers/               Business logic for auth and tests
  models/                    TAFFY-based models using JSON files
  data/                      JSON data (users, tests, results)
  config/config.js           Server config (secret, rightOptions)
  utils/                     Token helpers and middleware

frontend/
  index.html                 Main HTML shell
  templates/                 Page templates (test, choice, answers)
  styles/                    CSS files
  static/                    Fonts, images
  src/
    app.js                   Frontend bootstrap
    router.js                Client-side routing
    components/              UI components (Form, Test, Result, etc.)
    services/                HTTP and auth services
    utils/                   Helper utilities
  webpack.config.js          Webpack configuration
```

## Getting started

### Prerequisites

- Node.js (recommended v16 or later)
- npm (or yarn)
- A terminal / command prompt

### Installation

1. Clone the repository:

```
git clone https://github.com/GlacierAD/Quiz.git
cd Quiz
```

2. Install backend dependencies:

```
cd backend
npm install
```

3. Install frontend dependencies:

```
cd ../frontend
npm install
```
   
## Running the backend

From the `backend` directory:

```
node app.js
```
The backend runs on http://localhost:3000.

Routes:
- Authentication: `/api/login`, `/api/signup`, `/api/refresh`, `/api/logout`
- Tests: `/api/tests`, `/api/tests/:id`, `/api/tests/:id/pass`, `/api/tests/:id/result`, etc.

## Running the frontend (development)

From the `frontend` directory:

```
npx webpack serve --config webpack.config.js --mode development
```

Or, if an npm script exists:

```
npm run dev
```

The frontend runs on http://localhost:9000.

It communicates with the backend at:

```
http://localhost:3000/api
```

This value is defined in `frontend/config/config.js`.

## Building the frontend for production

From the `frontend` directory:

```
npx webpack --config webpack.config.js --mode production
```

The production build is generated in:

```
frontend/dist/
```

Serve this folder using any static server.

## Configuration

### Backend

`backend/config/config.js` contains:
- `secret` — JWT secret (should be moved to environment variables in real deployments)
- `rightOptions` — mapping of correct answers for each test

Example:

```
module.exports = {
  secret: 'your-secret-here',
  rightOptions: { ... }
}
```

### Frontend

`frontend/config/config.js` contains:

```
export default { host: 'http://localhost:3000/api' }
```

Change this if your backend runs on a different host or port.

## API summary

### Authentication

**POST /api/signup**  
Registers a new user
Body: `{ name, email, password }`

**POST /api/login**  
Authenticates user
Body: `{ email, password }`  
Returns: access token, refresh token

**POST /api/refresh**  
Refreshes access token
Body: `{ refreshToken }`

**POST /api/logout**  
Clears refresh token
Body: `{ email }`

### Tests

**GET /api/tests**  
Returns list of tests

**GET /api/tests/:id**  
Returns test details

**POST /api/tests/:id/pass**  
Submits answers
Body: `{ userId, results: [...] }`  
Returns: `{ score, total }`

**GET /api/tests/:id/result?userId=...**  
Returns user’s score

**GET /api/tests/:id/result/details?userId=...**  
Returns detailed results including correct answers

## Data and persistence

- Data is stored in JSON files under `backend/data/`
- TAFFY provides in‑memory querying
- Persistence is mocked and not suitable for production
- Typical files:
  - `users.json`
  - `tests.json`
  - `test-results.json`
  
Correct answers are defined in `config.rightOptions`.