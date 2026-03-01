# Film Finder — Discover Movies & TV Shows, Fast

A modern Angular application to discover trending movies and TV shows, browse categories, search by title, and view detailed information powered by TMDb. Built with Angular standalone components, RxJS, and Bootstrap, it features a clean architecture, typed models, and a secure token flow via Netlify Functions.

[**View Live Demo**](https://uyarmelik-film-finder.netlify.app) &nbsp;&nbsp; <a href="https://app.netlify.com/projects/uyarmelik-film-finder/deploys"><img src="https://api.netlify.com/api/v1/badges/b3ae9f9b-be79-4e98-9e60-26d69c21e7c0/deploy-status" alt="Netlify Status" align="center"></a>

## What It Does

Film Finder is a single-page app that lets you discover movies and TV shows using the TMDb API. You can browse trending content, explore categories (Movies, TV Shows), search by title, and open detail pages with banners and metadata. The app gets a Bearer token from Netlify Functions so the TMDb key stays server-side.

## Features

- Single-page app bootstrapped by [`AppComponent`](src/app/app.component.ts) with providers from [`appConfig`](src/app/app.config.ts) and routes from [`routes`](src/app/app.routes.ts)
- Data from TMDb via [`GenericHttpService`](src/app/services/generic-http.service.ts), which obtains the Bearer token using [`ConfigService`](src/app/services/config.service.ts)
- **Home** ([`HomeComponent`](src/app/pages/home/home.component.ts)): Trends, Movies, and TV Shows with pagination; switch segments via [`SegmentedControlComponent`](src/app/components/segmented-control/segmented-control.component.ts); “More Result” for more items
- **Categories** ([`ViewCategoryComponent`](src/app/pages/view-category/view-category.component.ts)): discovery and search with infinite “More Result”
- **Details** ([`DetailComponent`](src/app/pages/detail/detail.component.ts)): banners and metadata for movies or TV shows
- UI components: [`NavBarComponent`](src/app/components/nav-bar/nav-bar.component.ts), [`SegmentedControlComponent`](src/app/components/segmented-control/segmented-control.component.ts), [`MovieCardComponent`](src/app/components/movie-card/movie-card.component.ts), [`RateChipComponent`](src/app/components/rate-chip/rate-chip.component.ts), [`DetailBannerComponent`](src/app/components/detail-banner/detail-banner.component.ts), [`InputComponent`](src/app/components/input/input.component.ts)
- API endpoints centralized in [`Endpoints`](src/app/endpoints/endpoints.ts) (e.g. `TRENDS`, `MOVIES`, `TV_SHOWS`, `SEARCH_MOVIES`, `SEARCH_TV_SHOWS`, `MOVIE_ID`, `TV_SHOW_ID`, `IMAGE_BASE`)

## Tech Stack

- Angular 18 (Standalone Components & provideRouter)
- TypeScript (ES2022, strict mode)
- RxJS for async streams
- Bootstrap 5 and Bootstrap Icons
- SCSS theming: [src/themes/theme.scss](src/themes/theme.scss), global styles: [src/styles.scss](src/styles.scss)
- Netlify Functions for secret management: [netlify/functions/getToken.js](netlify/functions/getToken.js)
- TMDb v3 API
- Unit tests: Karma + Jasmine

## Project Structure

```
src/
  app/
    app.component.ts, app.config.ts, app.routes.ts
    pages/          → home, detail, view-category
    components/     → nav-bar, segmented-control, movie-card, rate-chip, detail-banner, input
    services/       → generic-http.service.ts, config.service.ts
    endpoints/      → endpoints.ts
    interfaces/     → models, ui-config
  environments/     → environment.prod.ts (and environment.ts for local dev)
  themes/           → theme.scss
  styles.scss
netlify/
  functions/        → getToken.js
```

## Prerequisites

- Node.js >= 18
- npm >= 9 (or yarn/pnpm)
- Angular CLI: `npm i -g @angular/cli`
- TMDb v4 Read Access Token

## Local Development

1. Clone the repo and install dependencies:

   ```sh
   npm install
   # or: yarn install
   ```

2. Configure the TMDb token (choose one):

   - **Netlify (recommended):** Set `TMDB_TOKEN` in Netlify environment variables. The client gets the token via [netlify/functions/getToken.js](netlify/functions/getToken.js) used by [`ConfigService`](src/app/services/config.service.ts) → [`GenericHttpService`](src/app/services/generic-http.service.ts).
   - **Local (without Netlify):** Create `src/environments/environment.ts` with:
     ```ts
     export const environment = { production: false, TMDB_TOKEN: '<your_v4_token>' };
     ```
     In [`GenericHttpService`](src/app/services/generic-http.service.ts), set `isNetlify = false`.

3. Start the dev server:

   ```sh
   npm run start
   # open http://localhost:4200/
   ```

   Or with auto-open: `npm run srv`.

   To test the serverless token flow locally, use Netlify CLI: `netlify dev` (app at http://localhost:8888, function at `/.netlify/functions/getToken`).

4. Production env is in [src/environments/environment.prod.ts](src/environments/environment.prod.ts). Do not commit real secrets.

## Scripts

| Script   | Command | Description |
| -------- | ------- | ----------- |
| `start`  | `ng serve` | Start dev server |
| `build`  | `ng build` | Production build |
| `watch`  | `ng build --watch --configuration development` | Build in watch mode |
| `test`   | `ng test` | Run unit tests (Karma + Jasmine) |
| `srv`    | `ng serve --port 4200 --open` | Dev server with auto-open browser |
| `dply`   | `ng build --configuration production --base-href https://uyarmelik.github.io/movie-web-app-angular/` | Production build for GitHub Pages deploy |

## Environment Variables

- **`TMDB_TOKEN`** — TMDb v4 Read Access Token.
  - **Production (Netlify):** Set in Netlify site environment variables. [netlify/functions/getToken.js](netlify/functions/getToken.js) reads `process.env.TMDB_TOKEN` and returns it to the client.
  - **Local development:** Set in `src/environments/environment.ts` as `environment.TMDB_TOKEN` when not using Netlify.

## Testing

Run unit tests with:

```sh
npm run test
```

Tests use Karma and Jasmine (configured in Angular `test` target in `angular.json`).

## Deployment

- **Netlify:** Connect the repo to Netlify; set `TMDB_TOKEN` in the site’s environment variables. Build command: `npm run build`; publish directory: `dist/` (or the app’s output path from `angular.json`). Live demo: [uyarmelik-film-finder.netlify.app](https://uyarmelik-film-finder.netlify.app).
- **GitHub Pages:** Use the `dply` script for a production build with the correct `--base-href`, then deploy the `dist/` output to GitHub Pages.

## License

See repository.
