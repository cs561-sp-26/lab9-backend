# SpeedScore --- Lab 9 Backend Starter

This repository contains the SpeedScore backend REST API (Express + MongoDB) at the end of Chapter 23 of *Full Stack Web Development from the Ground Up*. It is the starting point for **Lab 9: Deployment**.

## What's In Here

- Express server (`src/server.js`) with session management, rate limiting, CSRF protection, and Swagger docs
- REST routes for users (`/users`), rounds (`/rounds`), and authentication (`/auth`)
- Passport.js GitHub OAuth integration
- MongoDB + Mongoose models
- Jest test suite in `src/tests/`

## Getting Started Locally

```bash
npm install
```

Create a `.env` file at the repo root. Required variables:

```env
# Database
MONGODB_URI=mongodb+srv://<user>:<password>@<cluster>.mongodb.net/<dbname>

# Session and tokens
SESSION_SECRET=<long random string>
JWT_SECRET=<long random string>
ACCESS_TOKEN_DURATION=1h
REFRESH_TOKEN_DURATION=7d

# Server
PORT=3001
NODE_ENV=development
API_DEPLOYMENT_URL=http://localhost:3001
```

Optional --- enable email features (account verification, password reset):

```env
SENDGRID_API_KEY=your_sendgrid_api_key
SENDGRID_FROM_ADDRESS=noreply@yourdomain.com
```

Optional --- enable GitHub OAuth login:

```env
GITHUB_CLIENT_ID=your_github_oauth_app_client_id
GITHUB_CLIENT_SECRET=your_github_oauth_app_client_secret
```

Optional --- cross-origin cookie handling in production:

```env
COOKIE_DOMAIN=your-deployed-backend-domain.com
```

Start the dev server:

```bash
npm run dev
```

Swagger API documentation is available at `http://localhost:3001/api-docs`.

## Running Tests

```bash
npm test
```

## Known Deployment Gotchas

- **MongoDB Atlas network access** --- After creating a cluster, go to *Network Access* in Atlas and add `0.0.0.0/0` to the IP allowlist (or your hosting platform's specific IP range). Without this, the server cannot reach the database and crashes on startup.
- **File name case sensitivity** --- The server imports `./middleware/rateLimiter.js` (camelCase). On Linux hosts (Render, Railway, Fly.io, etc.) the file system is case-sensitive. If you encounter a `MODULE_NOT_FOUND` error on startup, check that the filename on disk matches the import exactly.
- **`NODE_ENV=production`** --- Set this on your hosting platform. It puts Express into production mode, which enables secure session cookies and suppresses verbose error output.
- **`API_DEPLOYMENT_URL`** --- Set this to your deployed backend URL (e.g., `https://my-speedscore-api.onrender.com`). It is used in GitHub OAuth callback URLs and email verification links.