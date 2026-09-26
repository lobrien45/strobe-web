# Strobe Web

Strobe is a platform for sharing photos with family and friends. It was developed for CAB432 Cloud Computing at the Queensland University of Technology, forked from an open-source Instagram-clone frontend by [@yassinjouao](https://github.com/yassinjouao).

## Features

- User authentication (login / signup)
- A home feed of posts from followed users
- Creating and sharing photo posts ("Share")
- Viewing an individual post in detail ("ShowPost")
- User profiles
- Search for other users
- A "Moments" feature alongside the main feed

## Tech Stack

- **Frontend:** React (Vite)
- **Routing:** React Router (`react-router-dom`)
- **HTTP client:** Axios
- **State management:** React Context + `useReducer` (see `AuthContext`)
- **Notifications:** [Sonner](https://sonner.emilkowal.ski/) toast notifications
- **Styling:** Plain CSS (`index.css`, component-level CSS for modals)

## Getting Started

### Prerequisites

- Node.js v24
- npm v11
- A running instance of the Strobe backend API (see [API Base URL](#api-base-url-important) below)

### Installation

```bash
git clone <repo-url>
cd strobe-web
npm install
```

### Running locally

```bash
npm run dev
```

The app will be available at `http://localhost:5173` (Vite's default port).

### Building for production

```bash
npm run build
```

## API Base URL (Important)

The client controls which backend API the browser calls. The default local API is `http://localhost:3000`.

Change it in one of these ways:

1. Copy `.env.example` to `.env` and set:
   ```
   VITE_API_BASE_URL=http://localhost:3000
   ```
   Restart `npm run dev` after changing `.env`.
2. Use the **Change API URL** dialog, available from the login/signup screen. This stores a runtime override in `localStorage` under the key `strobe_api_base_url`.

A stored runtime override takes priority over `.env` while the app is running. Changing `VITE_API_BASE_URL` and restarting invalidates old overrides automatically. Use **Use .env/default** on the login/signup screen to clear a current override.

## Authentication

Auth state is managed by `AuthContext` (`src/contexts/AuthContext/AuthContext.jsx`), using a reducer with two actions:

- `LOGIN_SUCCESS` — stores the logged-in user and auth token, persisted to `localStorage` (`strobe_user`, `strobe_token`).
- `LOGOUT` — clears user and token from both state and `localStorage`.

The Axios instance in `src/api.js` automatically attaches the stored token as a `Bearer` header on every outgoing request, so authenticated API calls don't need to set this manually per-request.

## Routing

Defined in `src/App.jsx`. All routes redirect to `/login` if there is no authenticated user, and away from `/login`/`/register` if there already is one:

| Route | Page | Notes |
|---|---|---|
| `/` | `Home` | Main feed; requires auth |
| `/login` | `Login` | Redirects to `/` if already logged in |
| `/register` | `Signup` | Redirects to `/` if already logged in |
| `/profile/:userId` | `Profile` | Requires auth |
| `*` (any other path) | — | Redirects to `/` |

## Project Structure

```
src/
├── api.js                        # Axios instance, API base URL resolution/override logic
├── App.jsx                        # Root component, route definitions
├── main.jsx                       # Application entry point
├── index.css                      # Global styles
├── contexts/
│   └── AuthContext/
│       └── AuthContext.jsx        # Auth state (user, token) via useReducer
└── components/
    ├── Feed.jsx                    # Main post feed
    ├── Post.jsx                    # Individual post display
    ├── ShowPost.jsx                # Detailed single-post view
    ├── Share.jsx                   # Create/share a new post
    ├── Moments.jsx                 # Moments feature
    ├── Search.jsx                  # User search
    ├── SearchBarMobile.jsx         # Mobile variant of search
    ├── Rightbar.jsx                # Sidebar content
    ├── Topbar.jsx                  # Top navigation bar
    ├── UI/
    │   ├── Modal.jsx               # Reusable modal component
    │   ├── Modal.css
    │   └── Backdrop.jsx            # Modal backdrop overlay
    └── pages/
        ├── Home.jsx                # Home page (feed + sidebars)
        ├── Login.jsx                # Login page
        ├── Signup.jsx               # Registration page
        └── Profile.jsx               # User profile page
```

## Known Issues / Limitations

[Add known limitations here once issues are seeded, so this section stays consistent with the issue tracker.]

## Authors

- [@yassinjouao](https://github.com/yassinjouao) — original `instagram-clone-frontend`
- Jackson Riding, QUT CAB432 staff

## License

MIT — see `LICENSE.md`.
