# tasks

A set of four small test assignments and exercises in JavaScript, Node.js and PHP.

## Context

Written in November 2020 (7-11 November) as small test assignments and practice tasks covering frontend JavaScript, Node.js and PHP. Each task is independent and lives in its own folder. The repository is kept as a snapshot of that time; in 2026 only two small fixes were made (the weather API key is now read from the environment, and the CSV import page got a neutral title).

## Features

- **Square game** (`js/square`): click a square of random size, position and colour as many times as possible within a configurable time (5 seconds minimum); the score is shown at the end.
- **Blog** (`js/blog`): single-page blog in plain JavaScript (no framework).
  - Create posts of type "note" or "news" with form validation (required title, text of at least 10 characters).
  - List posts and open a single post.
  - Add or remove posts from favourites, stored in `localStorage`.
  - Posts are stored through the Firebase Realtime Database REST API.
- **Weather** (`node_js/weather`): Express app with a form that takes a city name and shows the current temperature in degrees Celsius from the OpenWeather API.
- **CSV import** (`php/csv_import`): upload a CSV file with `article;price;count` lines into a MySQL table.
  - Existing articles are updated, new ones inserted.
  - Articles missing from the uploaded file get their count set to 0.
  - Export of the table to CSV, sorted by count.
  - A modal lists the records changed by the last import.

## Tech stack

- JavaScript (ES2015+ modules, classes, async/await), HTML, CSS
- Webpack 5, Babel 7, webpack-dev-server (blog)
- Node.js, Express 4, EJS, request-promise, nodemon (weather)
- PHP with mysqli, MySQL, Bootstrap, jQuery (CSV import)

## Architecture

```
js/square/        static page: index.html, index.js, styles.css
js/blog/          webpack project
  src/core/       base Component class, Form helper, Validators
  src/components/ header, navigation, posts, create, favourites, loader
  src/services/   API service (fetch) and data transformation
  src/templates/  post template
  dist/           built bundle (committed)
node_js/weather/  index.js (Express routes), requests/weather.request.js (OpenWeather call), views/index.ejs
php/csv_import/   index.php (Import class and page), asset/ (Bootstrap, jQuery, sample file)
```

Key decisions:

- The blog is built from small component classes that inherit from a common `Component` base class, with the API access in a separate service instead of inside the components.
- The weather app keeps the HTTP call to OpenWeather in its own module and passes only `weather` / `error` to the template.

## Getting started

Clone the repository, then start the task you are interested in.

**Square game** - no build step; open `js/square/index.html` in a browser.

**Blog**

```bash
cd js/blog
npm install
npm start          # webpack-dev-server in development mode
npm run build      # production build into dist/
```

A prebuilt version is in `js/blog/dist/index.html`.

**Weather** - requires an OpenWeather API key in the environment variable `OPENWEATHER_API_KEY`:

```bash
cd node_js/weather
npm install
OPENWEATHER_API_KEY=<your-key> npm run prod    # or: npm start (nodemon)
```

The app listens on http://localhost:3000.

**CSV import** - requires PHP with mysqli and a MySQL database with a table `test` (columns `ARTICUL`, `PRICE`, `COUNT`). The repository has no schema file. Set the connection details in `Import::$_settings` at the top of `php/csv_import/index.php`, then serve the folder, for example:

```bash
cd php/csv_import
php -S localhost:8000
```

## Testing

There are no automated tests.

## Status

Archived. Written in November 2020 and kept for history; no further development is planned. Known limitations from that time: the CSV import builds SQL by string concatenation instead of prepared statements and keeps its database settings in the code.

## License

MIT, see [LICENSE](LICENSE).
