# Conference Expense Planner

Live demo: https://felipeact.github.io/conference_event_planner/

Conference Expense Planner is a client-side React application for estimating the cost of a conference or other major event. Users choose venue rooms, add audio/visual equipment, select meals, enter the number of attendees, and review a combined event total.

## Features

- Venue selection with per-item quantities and prices.
- Audio/visual add-ons including projectors, speakers, microphones, whiteboards, and signage.
- Meal selection for breakfast, high tea, lunch, and dinner.
- Attendee count input used to calculate the cost of each selected meal.
- Section totals for venues, add-ons, and meals.
- A details view listing selected items, quantities, subtotals, and the total event cost.
- Client-side navigation between the venue, add-ons, and meals sections.

The application does not persist selections. Refreshing the page resets the Redux store to its initial values.

## Technology

- React 18.2 with React DOM.
- Vite 5 for development and production builds.
- Redux Toolkit 2.2 and React Redux 9.1 for application state.
- ESLint 8 with React, React Hooks, and React Refresh plugins.
- `gh-pages` for publishing the Vite build to GitHub Pages.

There is no backend service, database, HTTP API, authentication, or server-side persistence in this repository. The product catalog and pricing are defined in the Redux slices, and some catalog images are loaded from Pixabay URLs at runtime.

## Prerequisites

Install Node.js and npm. The repository does not declare a required Node.js version.

## Installation

Clone the repository and install the locked dependency tree:

```bash
git clone https://github.com/ibm-developer-skills-network/conference_event_planner.git
cd conference_event_planner
npm ci
```

`npm install` can also be used when updating dependencies or when no lockfile is available.

## Configuration

No environment variables or configuration files are required for local development. There are no `.env` files in the repository.

The Vite base path is set to `/conference_event_planner/` in `vite.config.js`. This is required for the published GitHub Pages URL. If deploying under a different path, update the `base` value before building.

## Run Locally

Start the Vite development server:

```bash
npm run dev
```

Open the local URL printed by Vite. The application starts on its introductory screen; select **Get Started** to open the planner.

To serve a production build locally:

```bash
npm run preview
```

The `preview` script runs `vite build` and then starts `vite preview --host`.

## Using The Planner

1. Select **Get Started** on the introductory screen.
2. In **Venue Room Selection**, use `+` and `-` to adjust room quantities.
3. In **Add-ons Selection**, adjust quantities for the required equipment and services.
4. In **Meals Selection**, enter the number of people and check the meals to include.
5. Review each section's subtotal. Select **Show Details** to see the selected items and the total event cost.

The venue catalog is defined in `src/venueSlice.js`:

| Venue | Unit cost | Capacity |
| --- | ---: | ---: |
| Conference Room | $3,500 | 15 |
| Auditorium Hall | $5,500 | 200 |
| Presentation Room | $700 | 50 |
| Large Meeting Room | $900 | 10 |
| Small Meeting Room | $1,100 | 5 |

The auditorium quantity is limited to three. Other venue controls disable visually at a quantity of ten, although the reducer itself does not enforce that ten-item limit. Meal prices are per person; venue and add-on prices are multiplied by their selected quantities.

## State And Component Architecture

`src/main.jsx` mounts the React application and wraps it in the Redux `Provider`. `src/store.js` combines three slices:

- `venue`: venue catalog and `incrementQuantity`/`decrementQuantity` actions.
- `av`: audio/visual catalog and `incrementAvQuantity`/`decrementAvQuantity` actions.
- `meals`: meal catalog and the `toggleMealSelection` action.

`App.jsx` renders the introductory view and reveals `ConferenceEvent.jsx` after **Get Started**. `ConferenceEvent.jsx` reads the three Redux collections, dispatches selection changes, calculates section totals, and passes totals to `TotalCost.jsx`. Styling is split between `src/App.css`, `src/ConferenceEvent.css`, `src/TotalCost.css`, and `src/index.css`.

## Project Structure

```text
.
├── index.html                 # HTML entrypoint and document title
├── package.json               # Scripts and dependencies
├── package-lock.json          # Locked npm dependency versions
├── vite.config.js             # React plugin and GitHub Pages base path
├── public/                    # Static files available at the site root
└── src/
	├── main.jsx               # React and Redux entrypoint
	├── App.jsx                # Intro screen and planner visibility
	├── ConferenceEvent.jsx    # Planner controls and cost calculations
	├── TotalCost.jsx          # Selected-items and total-cost display
	├── AboutUs.jsx            # Introductory content
	├── store.js               # Redux store
	├── venueSlice.js          # Venue state and actions
	├── avSlice.js             # AV add-on state and actions
	├── mealsSlice.js          # Meal state and actions
	└── *.css                  # Application styles
```

## Testing And Quality Checks

No automated test framework or `test` npm script is configured. The available checks are:

```bash
npm run build
npm run lint
```

`npm run build` creates the production bundle in `dist/`. The current lint configuration is strict (`--max-warnings 0`); existing lint violations must be resolved before lint passes.

## Build And Deployment

Create the production bundle:

```bash
npm run build
```

The output is written to `dist/`, which is ignored by Git. To publish the build with the configured `gh-pages` script, authenticate with GitHub and ensure you have permission to push to the repository, then run:

```bash
npm run deploy
```

`npm run deploy` first runs the `predeploy` hook (`npm run build`) and then publishes `dist/` using `gh-pages -d dist`. The configured Vite base path and the existing live demo assume the GitHub Pages project path `/conference_event_planner/`.

## Available npm Scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the Vite development server. |
| `npm run build` | Build the production assets in `dist/`. |
| `npm run lint` | Run ESLint with zero warnings allowed. |
| `npm run preview` | Build and serve the production bundle with Vite Preview. |
| `npm run deploy` | Build through `predeploy` and publish `dist/` to GitHub Pages. |

## License

See [LICENSE](LICENSE).