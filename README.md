# Mesto Microfrontend Architecture with Module Federation

## Phase 1: Framework Decision

For modularizing the Mesto application, we chose **Webpack 5 Module Federation** — a modern, flexible way to compose microfrontends dynamically at runtime.

### Why Module Federation?
- **Seamless Webpack integration** — no need for additional frameworks
- **Dynamic runtime loading** of independent applications
- **Shared dependencies** like `react`, `react-dom`, and `react-router-dom` reduce bundle size and prevent version conflicts
- **Minimal boilerplate** — straightforward to implement and reason about
- **Highly scalable**, perfect for gradual adoption in large teams

## Phase 2: Application Decomposition

We’ve broken down the Mesto frontend into four independently deployable microfrontends:

### 1. `profile-app`
**Responsibilities:**
- Create and edit user profile
- Display user bio and avatar

### 2. `gallery-app`
**Responsibilities:**
- Show photo gallery with cards
- Upload and delete images

### 3. `likes-app`
**Responsibilities:**
- Track and toggle likes on photo cards
- Display like counts

### 4. `shell-app` (host)
**Responsibilities:**
- Bootstrap and layout container
- Global routing and UI composition
- Load and render remote microfrontends

### Component Mapping Table

| Component                | Microfrontend    | Notes                                    |
|-------------------------|------------------|------------------------------------------|
| `Profile`               | `profile-app`    | User name, bio, avatar                   |
| `EditProfileForm`       | `profile-app`    | Form modal for profile editing           |
| `CardList`              | `gallery-app`    | Grid display of photo cards              |
| `Card`                  | `gallery-app`    | Single photo card                        |
| `AddPhotoPopup`         | `gallery-app`    | Modal for uploading new photo            |
| `LikeButton`            | `likes-app`      | Like toggle logic                        |
| `LikeCounter`           | `likes-app`      | Display number of likes                  |
| `AppRouter`             | `shell-app`      | Route definitions and lazy MFE loading   |
| `Header`, `Footer`      | `shell-app`      | Global layout and navigation             |

## Project Directory Structure

```
mesto-mfe/
├── shell-app/              # Main host container
├── profile-app/            # Microfrontend for profile features
├── gallery-app/            # Microfrontend for image features
├── likes-app/              # Microfrontend for likes functionality
```

Each MFE contains:
- Isolated Webpack configuration with Module Federation
- Independent `package.json` and dependencies
- Dedicated dev server (`webpack-dev-server`)
- Independent entry points and routing where needed

## Phase 3: Local Dev & Runtime Flow

1. **Run each app locally:**
    - `shell-app`: http://localhost:3000
    - `profile-app`: http://localhost:3001
    - `gallery-app`: http://localhost:3002
    - `likes-app`: http://localhost:3003

2. **Remote integration:**
    - `shell-app` pulls remote components using each MFE’s `remoteEntry.js`

3. **Route-based MFE loading with React Router:**
    - `/profile` → Loads `profile-app`
    - `/gallery` → Loads `gallery-app`
    - `/likes` → Loads `likes-app`