# Film Finder — Discover Movies & TV Shows, Fast



A modern Angular application to discover trending movies and TV shows, browse categories, search by title, and view detailed information powered by TMDb. Built with Angular standalone components, RxJS, and Bootstrap, it features a clean architecture, typed models, and a secure token flow via Netlify Functions.

[🚀 **View Live Demo**](https://uyarmelik-film-finder.netlify.app) &nbsp;&nbsp; <a href="https://app.netlify.com/projects/uyarmelik-film-finder/deploys"><img src="https://api.netlify.com/api/v1/badges/b3ae9f9b-be79-4e98-9e60-26d69c21e7c0/deploy-status" alt="Netlify Status" align="center"></a>

---

## Overview
- Single-page app bootstrapped by [`AppComponent`](src/app/app.component.ts) with providers from [`appConfig`](src/app/app.config.ts) and routes from [`routes`](src/app/app.routes.ts).
- Data is fetched from TMDb via a centralized service [`GenericHttpService`](src/app/services/generic-http.service.ts), which obtains the Bearer token using [`ConfigService`](src/app/services/config.service.ts).
- Core pages:
  - Home: [`HomeComponent`](src/app/pages/home/home.component.ts) shows Trends, Movies, and TV Shows with pagination.
  - Categories: [`ViewCategoryComponent`](src/app/pages/view-category/view-category.component.ts) supports discovery and search with infinite “More Result”.
  - Details: [`DetailComponent`](src/app/pages/detail/detail.component.ts) renders banners and metadata for movies or TV shows.
- UI components: [`NavBarComponent`](src/app/components/nav-bar/nav-bar.component.ts), [`SegmentedControlComponent`](src/app/components/segmented-control/segmented-control.component.ts), [`MovieCardComponent`](src/app/components/movie-card/movie-card.component.ts), [`RateChipComponent`](src/app/components/rate-chip/rate-chip.component.ts), [`DetailBannerComponent`](src/app/components/detail-banner/detail-banner.component.ts).
- Endpoints are centralized in [`Endpoints`](src/app/endpoints/endpoints.ts) (e.g., [`Endpoints.TRENDS`](src/app/endpoints/endpoints.ts), [`Endpoints.MOVIES`](src/app/endpoints/endpoints.ts), [`Endpoints.TV_SHOWS`](src/app/endpoints/endpoints.ts), [`Endpoints.SEARCH_MOVIES`](src/app/endpoints/endpoints.ts), [`Endpoints.SEARCH_TV_SHOWS`](src/app/endpoints/endpoints.ts), [`Endpoints.MOVIE_ID`](src/app/endpoints/endpoints.ts), [`Endpoints.TV_SHOW_ID`](src/app/endpoints/endpoints.ts), [`Endpoints.IMAGE_BASE`](src/app/endpoints/endpoints.ts)).

---

## Tech Stack
- Angular 18 (Standalone Components & provideRouter)
- TypeScript (ES2022, strict mode)
- RxJS for async streams
- Bootstrap 5 and Bootstrap Icons
- SCSS theming: [src/themes/theme.scss](src/themes/theme.scss), global styles: [src/styles.scss](src/styles.scss)
- Netlify Functions for secret management: [netlify/functions/getToken.js](netlify/functions/getToken.js)
- TMDb v3 API
- Unit tests: Karma + Jasmine

---

## Installation
Prerequisites:
- Node.js >= 18
- npm >= 9 (or yarn/pnpm)
- Angular CLI: npm i -g @angular/cli
- TMDb v4 Read Access Token

Steps:
1) Clone and install
```sh
npm install
# or
yarn install
```

2) Configure TMDB token (choose one):
- Netlify (recommended):
  - Set TMDB_TOKEN in Netlify environment variables.
  - Client fetches it via [netlify/functions/getToken.js](netlify/functions/getToken.js) used by [`ConfigService`](src/app/services/config.service.ts) → [`GenericHttpService`](src/app/services/generic-http.service.ts).
- Local environment (development without Netlify):
  - Create src/environments/environment.ts with:
    ```ts
    export const environment = { production: false, TMDB_TOKEN: '<your_v4_token>' };
    ```
  - In [`GenericHttpService`](src/app/services/generic-http.service.ts), set `isNetlify = false`.

Notes:
- Production env file: [src/environments/environment.prod.ts](src/environments/environment.prod.ts).
- Do not commit real secrets.

---

## Usage
- Start dev server:
```sh
npm run start
# open http://localhost:4200/
```

- Alternative with auto-open:
```sh
npm run srv
```

- Netlify local (to test the serverless token flow):
```sh
# requires Netlify CLI
netlify dev
# App: http://localhost:8888
# Function: /.netlify/functions/getToken
```

- Build:
```sh
npm run build
```

In-app navigation:
- Top bar: [`NavBarComponent`](src/app/components/nav-bar/nav-bar.component.ts)
- Home: [`HomeComponent`](src/app/pages/home/home.component.ts)
  - Switch segments via [`SegmentedControlComponent`](src/app/components/segmented-control/segmented-control.component.ts)
  - Paginate with “More Result”
- Categories: [`ViewCategoryComponent`](src/app/pages/view-category/view-category.component.ts)
  - Search via [`InputComponent`](src/app/components/input/input.component.ts)
- Details: [`DetailComponent`](src/app/pages/detail/detail.component.ts)
  - Banner and metadata via [`DetailBannerComponent`](src/app/components/detail-banner/detail-banner.component.ts) and [`RateChipComponent`](src/app/components/rate-chip/rate-chip.component.ts)
