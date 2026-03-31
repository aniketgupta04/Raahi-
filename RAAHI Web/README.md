# RAAHI Web

RAAHI is a tourist safety platform with a Node.js/Express backend and a React + Vite frontend. The current codebase includes authentication flows, emergency and AI routes, map integrations, Firebase support, and a restructured landing/login experience.

## Structure

```text
RAAHI Web/
├── RAAHI Backend/
│   ├── config/              # env, MongoDB, Firebase, API key loaders
│   ├── controllers/         # backend controllers
│   ├── middleware/          # auth, validation, security helpers
│   ├── models/              # Mongoose models
│   ├── routes/              # auth, AI, emergency, geofences, users
│   ├── scripts/             # db, maintenance, examples, tests
│   └── server.js            # backend entry point
└── RAAHI Frontend/
    ├── src/
    │   ├── components/      # app UI and shared components
    │   ├── components/landing/
    │   ├── contexts/        # auth and Google Maps context
    │   ├── data/            # landing page content
    │   ├── layouts/         # route-level layout wrappers
    │   ├── pages/           # landing, login, dashboard placeholders
    │   ├── services/        # axios API layer and AI helpers
    │   └── main.jsx         # frontend entry point
    └── vite.config.js       # Vite + Tailwind + client key loading
```

## Current Frontend Flow

- `/` shows the landing page built from reusable React components
- `Access System` goes to `/login`
- `Emergency Access` currently goes to `/dashboard`
- `Learn How It Works` scrolls to the workflow section
- `/login` is a focused portal-style login screen
- `/dashboard` is currently a placeholder route for the next dashboard build-out

## Key Features

- React 19 frontend with Vite and Tailwind CSS
- Express backend with MongoDB and Mongoose
- JWT-based authentication endpoints
- Firebase client config on the frontend
- Firebase Admin setup on the backend
- Google Maps integration through a shared React context
- Gemini AI backend service with fallback responses when not configured
- OpenWeather usage in the frontend home/weather experience
- Panic/emergency flows and geofence-related backend routes

## API Key Setup

Real keys are no longer meant to live in source files or committed templates.

The project now uses file-based key loading with paths stored in `.env`.

### Shared external key folder

```text
D:\MY FILES\Raahi\API Keys
```

### Backend `.env`

File: `RAAHI Backend/.env`

```env
API_KEYS_FILE_PATH=D:\MY FILES\Raahi\API Keys\service-keys.json
FIREBASE_SERVICE_ACCOUNT_PATH=D:\MY FILES\Raahi\API Keys\raahi-adf39-firebase-adminsdk-fbsvc-89a7250842.json
```

### Frontend `.env`

File: `RAAHI Frontend/.env`

```env
VITE_API_KEYS_FILE_PATH=D:\MY FILES\Raahi\API Keys\client-keys.json
```

### Expected key files

`service-keys.json`

```json
{
  "geminiApiKey": "your_gemini_api_key_here"
}
```

`client-keys.json`

```json
{
  "firebase": {
    "apiKey": "your_firebase_api_key",
    "authDomain": "your_project.firebaseapp.com",
    "databaseURL": "https://your_project-default-rtdb.firebaseio.com",
    "projectId": "your_project_id",
    "storageBucket": "your_project.firebasestorage.app",
    "messagingSenderId": "your_sender_id",
    "appId": "your_app_id"
  },
  "googleMapsApiKey": "your_google_maps_api_key_here",
  "geminiApiKey": "your_gemini_api_key_here",
  "openWeatherApiKey": "your_openweather_api_key_here"
}
```

Notes:

- The external `D:` folder is outside the repo, so real keys are not pushed to GitHub
- Backend Firebase uses the service-account JSON file path
- Frontend Firebase, Maps, Gemini fallback, and OpenWeather are loaded through the client key JSON during Vite startup

## Environment Setup

### Backend

Required core values in `RAAHI Backend/.env`:

```env
PORT=3000
NODE_ENV=development
MONGODB_URI=your_mongodb_uri
JWT_SECRET=your_jwt_secret
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

### Frontend

Required core values in `RAAHI Frontend/.env`:

```env
VITE_API_BASE_URL=http://localhost:3000/api
VITE_API_KEYS_FILE_PATH=D:\MY FILES\Raahi\API Keys\client-keys.json
VITE_APP_NAME=RAAHI - Tourist Safety App
VITE_APP_VERSION=1.0.0
```

## Running Locally

### Backend

```bash
cd "RAAHI Web/RAAHI Backend"
npm install
npm run dev
```

Backend runs on `http://localhost:3000`.

### Frontend

```bash
cd "RAAHI Web/RAAHI Frontend"
npm install
npm run dev
```

Frontend runs on `http://localhost:5173`.

## Main Backend Route Groups

- `/api/auth`
- `/api/users`
- `/api/destinations`
- `/api/ai`
- `/api/emergency`
- `/api/geofences`

## Important Project Updates

- MongoDB config was centralized so the URI is managed from backend config instead of scattered scripts
- Firebase backend now supports service-account JSON file loading
- Frontend landing page was rebuilt into reusable routed components
- Login page was rebuilt as a focused portal route
- API keys were moved out of source code into external file-based configuration
- In-repo API key template folders were removed to avoid accidental GitHub exposure

## Current Limitations

- `/dashboard` is still a placeholder route in the new routed frontend structure
- Gemini is optional and will fall back gracefully if `geminiApiKey` is missing
- Some older docs in the repo may still describe previous setup patterns

## Troubleshooting

- If Firebase is skipped on the backend, verify `FIREBASE_SERVICE_ACCOUNT_PATH`
- If maps fail on the frontend, verify `googleMapsApiKey` in `client-keys.json`
- If Gemini warns that it is not configured, add `geminiApiKey` to `service-keys.json`
- If MongoDB fails, verify `MONGODB_URI` and Atlas access settings
- If Vite cannot find keys, verify `VITE_API_KEYS_FILE_PATH` points to the external folder
