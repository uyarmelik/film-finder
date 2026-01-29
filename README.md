# Movie Web App (Angular) — Discover Movies & TV Shows, Fast.

A modern, responsive Angular application for discovering trending movies and TV shows, exploring categories, searching by title, and viewing rich details powered by TMDb. Built with Angular standalone components, RxJS, and Bootstrap, it features clean architecture, typed models, and a secure token flow via Netlify Functions.

Live demo:
- Netlify: https://movie-web-app-angular.netlify.app/home
- GitHub Pages build-ready (see scripts)

---

## Table of Contents
- [Highlights](#highlights)
- [Architecture Overview](#architecture-overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Setup & Installation](#setup--installation)
- [Configuration (TMDB Token)](#configuration-tmdb-token)
- [Running Locally](#running-locally)
- [Usage](#usage)
- [Available Scripts](#available-scripts)
- [Key Modules & Components](#key-modules--components)
- [API Endpoints](#api-endpoints)
- [Testing](#testing)
- [Roadmap](#roadmap)
- [License](#license)

---

## Highlights
- Blazing-fast Angular 18 app using standalone components.
- Strongly-typed API models for robust data handling.
- Token management with Netlify Functions to keep secrets out of the client.
- Responsive, themeable UI using SCSS and Bootstrap.
- Clean routing and navigation across Home, Categories, and Details.

---

## Architecture Overview
- Routing is defined in [`routes`](src/app/app.routes.ts) and bootstrapped via [`AppComponent`](src/app/app.component.ts).
- All data access goes through [`GenericHttpService`](src/app/services/generic-http.service.ts), which:
  - Retrieves a TMDB Bearer token via [`ConfigService`](src/app/services/config.service.ts)
  - Calls TMDB REST endpoints (base: https://api.themoviedb.org/3)
- Token flow options:
  - Netlify function: [`netlify/functions/getToken.js`](netlify/functions/getToken.js)
  - Environment file fallback: [`src/environments/environment.prod.ts`](src/environments/environment.prod.ts)
- UI is composed of small, reusable standalone components:
  - Cards, chips, segmented control, search input, and banners.
- Centralized endpoint constants in [`Endpoints`](src/app/endpoints/endpoints.ts).

---

## Tech Stack
- Framework: Angular 18 (Standalone APIs)
- Language: TypeScript (ES2022 target)
- State/Async: RxJS
- UI: Bootstrap 5, Bootstrap Icons, SCSS theming
- Build/CLI: Angular CLI
- Hosting/Functions: Netlify (serverless functions)
- API: TMDb v3 (The Movie Database)

---

## Project Structure
```
src/
  app/
    app.component.ts
    app.routes.ts
    components/
      detail-banner/
      input/
      movie-card/
      nav-bar/
      rate-chip/
      segmented-control/
    endpoints/
      endpoints.ts
    interfaces/
      models/...
      ui-config/...
    pages/
      home/
      view-category/
      detail/
    services/
      config.service.ts
      generic-http.service.ts
  assets/
  environments/
  themes/
```

Key files:
- Routing: [src/app/app.routes.ts](src/app/app.routes.ts)
- Bootstrap config: [src/app/app.config.ts](src/app/app.config.ts)
- HTTP service: [src/app/services/generic-http.service.ts](src/app/services/generic-http.service.ts)
- Token service: [src/app/services/config.service.ts](src/app/services/config.service.ts)
- Endpoints: [src/app/endpoints/endpoints.ts](src/app/endpoints/endpoints.ts)
- Environments: [src/environments/environment.prod.ts](src/environments/environment.prod.ts)
- Netlify function: [netlify/functions/getToken.js](netlify/functions/getToken.js)

---

## Setup & Installation
Prerequisites:
- Node.js >= 18
- npm >= 9 (or yarn/pnpm)
- Angular CLI: npm i -g @angular/cli
- TMDB API Read Access Token (v4)

Install dependencies:
```bash
npm install
# or
yarn install
```

---

## Configuration (TMDB Token)
There are two supported ways to provide your TMDB token:

1) Netlify (recommended for production)
- Add environment variable TMDB_TOKEN in Netlify:
  - Site settings → Environment variables → TMDB_TOKEN
- The client fetches it via Netlify Function:
  - [`getToken.js`](netlify/functions/getToken.js) → [`ConfigService`](src/app/services/config.service.ts) → [`GenericHttpService`](src/app/services/generic-http.service.ts)

2) Local environment file (for local dev without Netlify)
- Create src/environments/environment.ts (ignored by Git) with:
```ts
export const environment = {
  production: false,
  TMDB_TOKEN: '<your_v4_read_access_token>'
};
```
- In [`GenericHttpService`](src/app/services/generic-http.service.ts), set isNetlify = false to use the environment file.

Note: [`environment.prod.ts`](src/environments/environment.prod.ts) is available if you prefer to place the token there for production builds. Never commit real secrets.

---

## Running Locally
Dev server:
```bash
npm run start
# then open http://localhost:4200/
```

Alternative dev server on a custom port:
```bash
npm run srv
```

Optional: Run with Netlify dev to use the serverless token flow locally:
```bash
# Requires Netlify CLI: npm i -g netlify-cli
netlify dev
# App: http://localhost:8888
# Function: /.netlify/functions/getToken
```

Build (production):
```bash
npm run build
```

---

## Usage
- Home
  - The default landing page is [`HomeComponent`](src/app/pages/home/home.component.ts) showing “Trends”.
  - Switch between Trends, Movies, and TV Shows with [`SegmentedControlComponent`](src/app/components/segmented-control/segmented-control.component.ts).
  - Infinite pagination via the “More Result” button.

- Categories
  - Navigate to /movies or /tvshows to see category listings powered by [`ViewCategoryComponent`](src/app/pages/view-category/view-category.component.ts).
  - Use the search input to query by title across the current category.

- Details
  - Click any card to view full details with banner, tagline, ratings, and metadata in [`DetailComponent`](src/app/pages/detail/detail.component.ts).

Navigation is provided by the top bar: [`NavBarComponent`](src/app/components/nav-bar/nav-bar.component.ts).

---

## Available Scripts
Defined in [package.json](package.json):

- Start dev server:
```bash
npm run start
```

- Build (default production configuration):
```bash
npm run build
```

- Watch build (development):
```bash
npm run watch
```

- Run tests:
```bash
npm run test
```

- Dev server on 4200 and open browser:
```bash
npm run srv
```

- GitHub Pages build with base-href:
```bash
npm run dply
```

---

## Key Modules & Components
- Services
  - HTTP: [`GenericHttpService`](src/app/services/generic-http.service.ts)
  - Config/Token: [`ConfigService`](src/app/services/config.service.ts)

- Routing & Bootstrap
  - Routes: [`routes`](src/app/app.routes.ts)
  - Root component: [`AppComponent`](src/app/app.component.ts)

- Pages
  - Home: [`HomeComponent`](src/app/pages/home/home.component.ts)
  - Category Listing: [`ViewCategoryComponent`](src/app/pages/view-category/view-category.component.ts)
  - Details: [`DetailComponent`](src/app/pages/detail/detail.component.ts)

- UI Components
  - Movie card: [`MovieCardComponent`](src/app/components/movie-card/movie-card.component.ts)
  - Rating chip: [`RateChipComponent`](src/app/components/rate-chip/rate-chip.component.ts)
  - Segmented control: [`SegmentedControlComponent`](src/app/components/segmented-control/segmented-control.component.ts)
  - Detail banner: [`DetailBannerComponent`](src/app/components/detail-banner/detail-banner.component.ts)
  - Nav bar: [`NavBarComponent`](src/app/components/nav-bar/nav-bar.component.ts)
  - Search input: [`InputComponent`](src/app/components/input/input.component.ts)

- Theming
  - Global SCSS variables: [src/themes/theme.scss](src/themes/theme.scss)
  - Global styles: [src/styles.scss](src/styles.scss)

- Endpoints
  - Constants factory: [`Endpoints`](src/app/endpoints/endpoints.ts)

---

## API Endpoints
Backed by TMDb v3:
- Trends: `trending/all/day`, `trending/movie/day`, `trending/tv/day`
- Discovery: `discover/movie`, `discover/tv`
- Search: `search/movie`, `search/tv`
- Details: `movie/{movie_id}`, `tv/{series_id}`

The request base URL is configured in [`GenericHttpService`](src/app/services/generic-http.service.ts) as https://api.themoviedb.org/3 and uses Bearer auth:
```
Authorization: Bearer <TMDB_TOKEN>
```

---

## Testing
Run unit tests via Karma/Jasmine:
```bash
npm run test
```

Spec files live alongside components and services (e.g., `*.component.spec.ts`, `*.service.spec.ts`).

---

## Roadmap
- Performance
  - Caching of API responses and smarter pagination.
  - Skeleton/loading states and error boundaries.
- UX
  - Favorites/Watchlist with local storage and/or backend sync.
  - Filters (genres, year, rating).
  - Internationalization (i18n) and accessibility polishing.
- Content
  - Cast, crew, trailers, and recommendations on detail pages.
- Tooling
  - E2E tests (e.g., Playwright/Cypress).
  - CI workflows for build/test/deploy.
- Deployment
  - Optional Dockerfile for containerized dev and preview environments.

---

## License
This project uses the TMDb API but is not endorsed or certified by TMDb.
Please ensure compliance with TMDb’s terms when using the API and assets.
