                    ---
                    # Strobe Web

Strobe is a platform for sharing photos with family and friends. It was developed for CAB432 Cloud Computing at the Queensland University of Technology, forked from an open-source Instagram-clone frontend by [@yassinjouao](https://github.com/yassinjouao).

## Features

- User authentication (login / signup)
- A home feed of posts from followed users
- Creating and sharing photo posts ("Share")
- Viewing an individual post in detail ("ShowPost")
- User profiles

## Tech Stack

- **Frontend:** React (Vite)
- **Routing:** React Router (`react-router-dom`)
- **HTTP client:** Axios
- **State management:** React Context + `useReducer` (see `AuthContext`)
- **Notifications:** [Sonner](https://sonner.emilkowal.ski/) toast notifications
- **Styling:** Plain CSS (`index.css`, component-level CSS for modals)

## Getting Started

### Prerequisites

- Node.js v24+
- npm v11+
- A running instance of the Strobe backend API (see [API Base URL](#api-base-url-important) below)

### Installation

```bash
git clone https://github.com/lobrien45/strobe-web.git
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

- **Environment Variable Naming:** The documentation references `VITE_API_BASE_URL` but the project uses Vite, which requires environment variables to start with `VITE_`. However, the `.env.example` file might not explicitly show this prefix, causing confusion. Ensure `.env.example` includes the correct `VITE_API_BASE_URL` variable and update documentation if backend proxying changes the effective variable name.
- **Missing Version Constraints in Prerequisites:** The prerequisites state Node.js v24+ and npm v11+ but do not specify upper bounds or compatibility notes. Add tested version ranges (e.g., Node.js 24.x LTS, npm 11.x) and note any known incompatibilities with newer versions to prevent setup issues.
- **Incomplete Known Issues Section:** The Known Issues section currently states "N/A" but the project likely has limitations such as lack of image optimization, limited mobile responsiveness in certain components, or missing accessibility labels. Replace "N/A" with specific, documented limitations or confirm the section is accurate if no issues exist.
                    ---
