<p align="center">
  <img src="static/assets/images/logo-gh.svg" width="350">
</p>

<p align="center">
  <img src="https://badges.fw-web.space/github/license/muety/wakapi">
  <a href="https://liberapay.com/muety/"><img src="https://badges.fw-web.space/liberapay/receives/muety.svg?logo=liberapay"></a>
  <img src="https://wakapi.dev/api/badge/n1try/interval:any/project:wakapi?label=wakapi">
  <img src="https://badges.fw-web.space/github/languages/code-size/muety/wakapi">
  <a href="https://goreportcard.com/report/github.com/muety/wakapi"><img src="https://goreportcard.com/badge/github.com/muety/wakapi"></a>
  <a href="https://sonarcloud.io/dashboard?id=muety_wakapi"><img src="https://sonarcloud.io/api/project_badges/measure?project=muety_wakapi&metric=ncloc"></a>
</p>

<h3 align="center">A minimalist, self-hosted WakaTime-compatible backend for coding statistics.</h3>

<div align="center">
  <h3>
    <a href="https://wakapi.dev">Website</a>
    <span> | </span>
    <a href="#-features">Features</a>
    <span> | </span>
    <a href="#%EF%B8%8F-how-to-use">How to use</a>
    <span> | </span>
    <a href="https://github.com/muety/wakapi/issues">Issues</a>
    <span> | </span>
    <a href="https://github.com/muety">Contact</a>
  </h3>
</div>

<p align="center">
  <img src="static/assets/images/screenshot.webp" width="500px">
</p>

Installation instructions can be found below and in the [Wiki](https://github.com/muety/wakapi/wiki).

## 🚀 Features

* ✅ Free and open-source
* ✅ Built by developers for developers
* ✅ Statistics for projects, languages, editors, hosts and operating systems
* ✅ Badges
* ✅ Weekly E-Mail reports
* ✅ REST API
* ✅ Partially compatible with WakaTime
* ✅ WakaTime integration
* ✅ Support for Prometheus exports
* ✅ Lightning fast
* ✅ Self-hosted

## 🚧 Roadmap

Plans for the near future mainly include, besides usual improvements and bug fixes, a UI redesign as well as additional types of charts and statistics (see [#101](https://github.com/muety/wakapi/issues/101), [#76](https://github.com/muety/wakapi/issues/76), [#12](https://github.com/muety/wakapi/issues/12)). If you have feature requests or any kind of improvement proposals feel free to open an issue or share them in our [user survey](https://github.com/muety/wakapi/issues/82).

## ⌨️ How to use?

There are different options for how to use Wakapi, ranging from our hosted cloud service to self-hosting it. Regardless of which option choose, you will always have to do the [client setup](#-client-setup) in addition.

### ☁️ Option 1: Use [wakapi.dev](https://wakapi.dev)

If you want to try out a free, hosted cloud service, all you need to do is create an account and then set up your client-side tooling (see below).

### 📦 Option 2: Quick-run a release

```bash
$ curl -L https://wakapi.dev/get | bash
```

**Alternatively** using [eget](https://github.com/zyedidia/eget):
```bash
$ eget muety/wakapi
```

### 🐳 Option 3: Use Docker

```bash
# Create a persistent volume
$ docker volume create wakapi-data

$ SALT="$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w ${1:-32} | head -n 1)"

# Run the container
$ docker run -d \
  -p 3000:3000 \
  -e "WAKAPI_PASSWORD_SALT=$SALT" \
  -v wakapi-data:/data \
  --name wakapi \
  ghcr.io/muety/wakapi:latest
```

**Note:** By default, SQLite is used as a database. To run Wakapi in Docker with MySQL or Postgres, see [Dockerfile](https://github.com/muety/wakapi/blob/master/Dockerfile) and [config.default.yml](https://github.com/muety/wakapi/blob/master/config.default.yml) for further options.

If you want to run Wakapi on **Kubernetes**, there is [wakapi-helm-chart](https://github.com/andreymaznyak/wakapi-helm-chart) for quick and easy deployment.

### 🧑‍💻 Option 4: Compile and run from source

```bash
# Build and install
# Alternatively: go build -o wakapi
$ go install github.com/muety/wakapi@latest

# Get default config and customize
$ curl -o wakapi.yml https://raw.githubusercontent.com/muety/wakapi/master/config.default.yml
$ vi wakapi.yml

# Run it
$ ./wakapi -config wakapi.yml
```

**Note:** Check the comments in `config.yml` for best practices regarding security configuration and more.

💡 When running Wakapi standalone (without Docker), it is recommended to run it as a [SystemD service](etc/wakapi.service).

### 💻 Client setup

Wakapi relies on the open-source [WakaTime](https://github.com/wakatime/wakatime) client tools. In order to collect statistics for Wakapi, you need to set them up.

1. **Set up WakaTime** for your specific IDE or editor. Please refer to the respective [plugin guide](https://wakatime.com/plugins)
2. **Edit your local `~/.wakatime.cfg`** file as follows.

```ini
[settings]

# Your Wakapi server URL or 'https://wakapi.dev/api' when using the cloud server
api_url = http://localhost:3000/api

# Your Wakapi API key (get it from the web interface after having created an account)
api_key = 406fe41f-6d69-4183-a4cc-121e0c524c2b
```

Optionally, you can set up a [client-side proxy](https://github.com/muety/wakapi/wiki/Advanced-Setup:-Client-side-proxy) in addition.

## 🔧 Configuration options

You can specify configuration options either via a config file (default: `config.yml`, customizable through the `-c` argument) or via environment variables. Here is an overview of all options.

| YAML key / Env. variable                                                     | Default                                          | Description                                                                                                                                                              |
|------------------------------------------------------------------------------|--------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `env` /<br>`ENVIRONMENT`                                                     | `dev`                                            | Whether to use development- or production settings                                                                                                                       |
| `app.aggregation_time` /<br>`WAKAPI_AGGREGATION_TIME`                        | `0 15 2 * * *`                                   | Time of day at which to periodically run summary generation for all users                                                                                                |
| `app.report_time_weekly` /<br>`WAKAPI_REPORT_TIME_WEEKLY`                    | `0 0 18 * * 5`                                   | Week day and time at which to send e-mail reports                                                                                                                        |
| `app.leaderboard_generation_time` /<br>`WAKAPI_LEADERBOARD_GENERATION_TIME`  | `0 0 6 * * *,0 0 18 * * *`                       | One or multiple times of day at which to re-calculate the leaderboard                                                                                                    |
| `app.data_cleanup_time` /<br>`WAKAPI_DATA_CLEANUP_TIME`                      | `0 0 6 * * 0`                                    | When to perform data cleanup operations (see `app.data_retention_months`)                                                                                                |
| `app.import_batch_size` /<br>`WAKAPI_IMPORT_BATCH_SIZE`                      | `50`                                             | Size of batches of heartbeats to insert to the database during importing from external services                                                                          |
| `app.inactive_days` /<br>`WAKAPI_INACTIVE_DAYS`                              | `7`                                              | Number of days after which to consider a user inactive (only for metrics)                                                                                                |
| `app.heartbeat_max_age /`<br>`WAKAPI_HEARTBEAT_MAX_AGE`                      | `4320h`                                          | Maximum acceptable age of a heartbeat (see [`ParseDuration`](https://pkg.go.dev/time#ParseDuration))                                                                     |
| `app.custom_languages`                                                       | -                                                | Map from file endings to language names                                                                                                                                  |
| `app.avatar_url_template` /<br>`WAKAPI_AVATAR_URL_TEMPLATE`                  | (see [`config.default.yml`](config.default.yml)) | URL template for external user avatar images (e.g. from [Dicebear](https://dicebear.com) or [Gravatar](https://gravatar.com))                                            |
| `app.support_contact` /<br>`WAKAPI_SUPPORT_CONTACT`                          | `hostmaster@wakapi.dev`                          | E-Mail address to display as a support contact on the page                                                                                                               |
| `app.data_retention_months` /<br>`WAKAPI_DATA_RETENTION_MONTHS`              | `-1`                                             | Maximum retention period in months for user data (heartbeats) (-1 for unlimited)                                                                                         |
| `server.port` /<br> `WAKAPI_PORT`                                            | `3000`                                           | Port to listen on                                                                                                                                                        |
| `server.listen_ipv4` /<br> `WAKAPI_LISTEN_IPV4`                              | `127.0.0.1`                                      | IPv4 network address to listen on (leave blank to disable IPv4)                                                                                                          |
| `server.listen_ipv6` /<br> `WAKAPI_LISTEN_IPV6`                              | `::1`                                            | IPv6 network address to listen on (leave blank to disable IPv6)                                                                                                          |
| `server.listen_socket` /<br> `WAKAPI_LISTEN_SOCKET`                          | -                                                | UNIX socket to listen on (leave blank to disable UNIX socket)                                                                                                            |
| `server.listen_socket_mode` /<br> `WAKAPI_LISTEN_SOCKET_MODE`                | `0666`                                           | Permission mode to create UNIX socket with                                                                                                                               |
| `server.timeout_sec` /<br> `WAKAPI_TIMEOUT_SEC`                              | `30`                                             | Request timeout in seconds                                                                                                                                               |
| `server.tls_cert_path` /<br> `WAKAPI_TLS_CERT_PATH`                          | -                                                | Path of SSL server certificate (leave blank to not use HTTPS)                                                                                                            |
| `server.tls_key_path` /<br> `WAKAPI_TLS_KEY_PATH`                            | -                                                | Path of SSL server private key (leave blank to not use HTTPS)                                                                                                            |
| `server.base_path` /<br> `WAKAPI_BASE_PATH`                                  | `/`                                              | Web base path (change when running behind a proxy under a sub-path)                                                                                                      |
| `server.public_url` /<br> `WAKAPI_PUBLIC_URL`                                | `http://localhost:3000`                          | URL at which your Wakapi instance can be found publicly                                                                                                                  |
| `security.password_salt` /<br> `WAKAPI_PASSWORD_SALT`                        | -                                                | Pepper to use for password hashing                                                                                                                                       |
| `security.insecure_cookies` /<br> `WAKAPI_INSECURE_COOKIES`                  | `false`                                          | Whether or not to allow cookies over HTTP                                                                                                                                |
| `security.cookie_max_age` /<br> `WAKAPI_COOKIE_MAX_AGE`                      | `172800`                                         | Lifetime of authentication cookies in seconds or `0` to use [Session](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#Define_the_lifetime_of_a_cookie) cookies |
| `security.allow_signup` /<br> `WAKAPI_ALLOW_SIGNUP`                          | `true`                                           | Whether to enable user registration                                                                                                                                      |
| `security.disable_frontpage` /<br> `WAKAPI_DISABLE_FRONTPAGE`                | `false`                                          | Whether to disable landing page (useful for personal instances)                                                                                                          |
| `security.expose_metrics` /<br> `WAKAPI_EXPOSE_METRICS`                      | `false`                                          | Whether to expose Prometheus metrics under `/api/metrics`                                                                                                                |
| `db.host` /<br> `WAKAPI_DB_HOST`                                             | -                                                | Database host                                                                                                                                                            |
| `db.port` /<br> `WAKAPI_DB_PORT`                                             | -                                                | Database port                                                                                                                                                            |
| `db.socket` /<br> `WAKAPI_DB_SOCKET`                                         | -                                                | Database UNIX socket (alternative to `host`) (for MySQL only)                                                                                                            |
| `db.user` /<br> `WAKAPI_DB_USER`                                             | -                                                | Database user                                                                                                                                                            |
| `db.password` /<br> `WAKAPI_DB_PASSWORD`                                     | -                                                | Database password                                                                                                                                                        |
| `db.name` /<br> `WAKAPI_DB_NAME`                                             | `wakapi_db.db`                                   | Database name                                                                                                                                                            |
| `db.dialect` /<br> `WAKAPI_DB_TYPE`                                          | `sqlite3`                                        | Database type (one of `sqlite3`, `mysql`, `postgres`, `cockroach`)                                                                                                       |
| `db.charset` /<br> `WAKAPI_DB_CHARSET`                                       | `utf8mb4`                                        | Database connection charset (for MySQL only)                                                                                                                             |
| `db.max_conn` /<br> `WAKAPI_DB_MAX_CONNECTIONS`                              | `2`                                              | Maximum number of database connections                                                                                                                                   |
| `db.ssl` /<br> `WAKAPI_DB_SSL`                                               | `false`                                          | Whether to use TLS encryption for database connection (Postgres and CockroachDB only)                                                                                    |
| `db.automgirate_fail_silently` /<br> `WAKAPI_DB_AUTOMIGRATE_FAIL_SILENTLY`   | `false`                                          | Whether to ignore schema auto-migration failures when starting up                                                                                                        |
| `mail.enabled` /<br> `WAKAPI_MAIL_ENABLED`                                   | `true`                                           | Whether to allow Wakapi to send e-mail (e.g. for password resets)                                                                                                        |
| `mail.sender` /<br> `WAKAPI_MAIL_SENDER`                                     | `Wakapi <noreply@wakapi.dev>`                    | Default sender address for outgoing mails (ignored for MailWhale)                                                                                                        |
| `mail.provider` /<br> `WAKAPI_MAIL_PROVIDER`                                 | `smtp`                                           | Implementation to use for sending mails (one of [`smtp`, `mailwhale`])                                                                                                   |
| `mail.smtp.host` /<br> `WAKAPI_MAIL_SMTP_HOST`                               | -                                                | SMTP server address for sending mail (if using `smtp` mail provider)                                                                                                     |
| `mail.smtp.port` /<br> `WAKAPI_MAIL_SMTP_PORT`                               | -                                                | SMTP server port (usually 465)                                                                                                                                           |
| `mail.smtp.username` /<br> `WAKAPI_MAIL_SMTP_USER`                           | -                                                | SMTP server authentication username                                                                                                                                      |
| `mail.smtp.password` /<br> `WAKAPI_MAIL_SMTP_PASS`                           | -                                                | SMTP server authentication password                                                                                                                                      |
| `mail.smtp.tls` /<br> `WAKAPI_MAIL_SMTP_TLS`                                 | `false`                                          | Whether the SMTP server requires TLS encryption (`false` for STARTTLS or no encryption)                                                                                  |
| `mail.mailwhale.url` /<br> `WAKAPI_MAIL_MAILWHALE_URL`                       | -                                                | URL of [MailWhale](https://mailwhale.dev) instance (e.g. `https://mailwhale.dev`) (if using `mailwhale` mail provider)                                                   |
| `mail.mailwhale.client_id` /<br> `WAKAPI_MAIL_MAILWHALE_CLIENT_ID`           | -                                                | MailWhale API client ID                                                                                                                                                  |
| `mail.mailwhale.client_secret` /<br> `WAKAPI_MAIL_MAILWHALE_CLIENT_SECRET`   | -                                                | MailWhale API client secret                                                                                                                                              |
| `sentry.dsn` /<br> `WAKAPI_SENTRY_DSN`                                       | –                                                | DSN for to integrate [Sentry](https://sentry.io) for error logging and tracing (leave empty to disable)                                                                  |
| `sentry.enable_tracing` /<br> `WAKAPI_SENTRY_TRACING`                        | `false`                                          | Whether to enable Sentry request tracing                                                                                                                                 |
| `sentry.sample_rate` /<br> `WAKAPI_SENTRY_SAMPLE_RATE`                       | `0.75`                                           | Probability of tracing a request in Sentry                                                                                                                               |
| `sentry.sample_rate_heartbeats` /<br> `WAKAPI_SENTRY_SAMPLE_RATE_HEARTBEATS` | `0.1`                                            | Probability of tracing a heartbeat request in Sentry                                                                                                                     |
| `quick_start` /<br> `WAKAPI_QUICK_START`                                     | `false`                                          | Whether to skip initial boot tasks. Use only for development purposes!                                                                                                   |

### Supported databases

Wakapi uses [GORM](https://gorm.io) as an ORM. As a consequence, a set of different relational databases is supported.

* [SQLite](https://sqlite.org/) (_default, easy setup_)
* [MySQL](https://hub.docker.com/_/mysql) (_recommended, because most extensively tested_)
* [MariaDB](https://hub.docker.com/_/mariadb) (_open-source MySQL alternative_)
* [Postgres](https://hub.docker.com/_/postgres) (_open-source as well_)
* [CockroachDB](https://www.cockroachlabs.com/docs/stable/install-cockroachdb-linux.html) (_cloud-native, distributed, Postgres-compatible API_)

## 🔧 API endpoints

See our [Swagger API Documentation](https://wakapi.dev/swagger-ui).

### Generating Swagger docs

```bash
$ go install github.com/swaggo/swag/cmd/swag@latest
$ swag init -o static/docs
```

## 🤝 Integrations

### Prometheus export

You can export your Wakapi statistics to Prometheus to view them in a Grafana dashboard or so. Here is how.

```bash
# 1. Start Wakapi with the feature enabled
$ export WAKAPI_EXPOSE_METRICS=true
$ ./wakapi

# 2. Get your API key and hash it
$ echo "<YOUR_API_KEY>" | base64

# 3. Add a Prometheus scrape config to your prometheus.yml (see below)
```

#### Scrape config example

```yml
# prometheus.yml
# (assuming your Wakapi instance listens at localhost, port 3000)

scrape_configs:
  - job_name: 'wakapi'
    scrape_interval: 1m
    metrics_path: '/api/metrics'
    bearer_token: '<YOUR_BASE64_HASHED_TOKEN>'
    static_configs:
      - targets: ['localhost:3000']
```

#### Grafana

There is also a [nice Grafana dashboard](https://grafana.com/grafana/dashboards/12790), provided by the author of [wakatime_exporter](https://github.com/MacroPower/wakatime_exporter).

![](https://grafana.com/api/dashboards/12790/images/8741/image)

### WakaTime integration

Wakapi plays well together with [WakaTime](https://wakatime.com). For one thing, you can **forward heartbeats** from Wakapi to WakaTime to effectively use both services simultaneously. In addition, there is the option to **import historic data** from WakaTime for consistency between both services. Both features can be enabled in the _Integrations_ section of your Wakapi instance's settings page.

### GitHub Readme Stats integrations

Wakapi also integrates with [GitHub Readme Stats](https://github.com/anuraghazra/github-readme-stats#wakatime-week-stats) to generate fancy cards for you. Here is an example. To use this, don't forget to **enable public data** under [Settings -> Permissions](https://wakapi.dev/settings#permissions).

![](https://github-readme-stats.vercel.app/api/wakatime?username=n1try&api_domain=wakapi.dev&bg_color=2D3748&title_color=2F855A&icon_color=2F855A&text_color=ffffff&custom_title=Wakapi%20Week%20Stats&layout=compact)

<details>
<summary>Click to view code</summary>

```markdown
![](https://github-readme-stats.vercel.app/api/wakatime?username={yourusername}&api_domain=wakapi.dev&bg_color=2D3748&title_color=2F855A&icon_color=2F855A&text_color=ffffff&custom_title=Wakapi%20Week%20Stats&layout=compact)
```

</details>
<br>

### Github Readme Metrics integration

There is a [WakaTime plugin](https://github.com/lowlighter/metrics/tree/master/source/plugins/wakatime) for GitHub [Metrics](https://github.com/lowlighter/metrics/) that is also compatible with Wakapi. To use this, don't forget to **enable public data** under [Settings -> Permissions](https://wakapi.dev/settings#permissions).

Preview:

![](https://raw.githubusercontent.com/lowlighter/metrics/examples/metrics.plugin.wakatime.svg)

<details>
<summary>Click to view code</summary>

```yml
- uses: lowlighter/metrics@latest
  with:
    # ... other options
    plugin_wakatime: yes
    plugin_wakatime_token: ${{ secrets.WAKATIME_TOKEN }}      # Required
    plugin_wakatime_days: 7                                   # Display last week stats
    plugin_wakatime_sections: time, projects, projects-graphs # Display time and projects sections, along with projects graphs
    plugin_wakatime_limit: 4                                  # Show 4 entries per graph
    plugin_wakatime_url: http://wakapi.dev                    # Wakatime url endpoint
    plugin_wakatime_user: .user.login                         # User

```

</details>
<br>

## 👍 Best practices

It is recommended to use wakapi behind a **reverse proxy**, like [Caddy](https://caddyserver.com) or [nginx](https://www.nginx.com/), to enable **TLS encryption** (HTTPS).

However, if you want to expose your wakapi instance to the public anyway, you need to set `server.listen_ipv4` to `0.0.0.0` in `config.yml`.

## 🧪 Tests

### Unit tests


Unit tests are supposed to test business logic on a fine-grained level. They are implemented as part of the application, using Go's [testing](https://pkg.go.dev/testing?utm_source=godoc) package alongside [stretchr/testify](https://pkg.go.dev/github.com/stretchr/testify).

#### How to run

```bash
$ CGO_ENABLED=0 go test `go list ./... | grep -v 'github.com/muety/wakapi/scripts'` -json -coverprofile=coverage/coverage.out ./... -run ./...
```

### API tests

API tests are implemented as black box tests, which interact with a fully-fledged, standalone Wakapi through HTTP requests. They are supposed to check Wakapi's web stack and endpoints, including response codes, headers and data on a syntactical level, rather than checking the actual content that is returned.

Our API (or end-to-end, in some way) tests are implemented as a [Postman](https://www.postman.com/) collection and can be run either from inside Postman, or using [newman](https://www.npmjs.com/package/newman) as a command-line runner.

To get a predictable environment, tests are run against a fresh and clean Wakapi instance with a SQLite database that is populated with nothing but some seed data (see [data.sql](testing/data.sql)). It is usually recommended for software tests to be [safe](https://www.restapitutorial.com/lessons/idempotency.html), stateless and without side effects. In contrary to that paradigm, our API tests strictly require a fixed execution order (which Postman assures) and their assertions may rely on specific previous tests having succeeded.

#### Prerequisites (Linux only)

```bash
# 1. sqlite (cli)
$ sudo apt install sqlite  # Fedora: sudo dnf install sqlite

# 2. newman
$ npm install -g newman
```

#### How to run (Linux only)

```bash
$ ./testing/run_api_tests.sh
```

## 🤓 Developer notes

### Building web assets

To keep things minimal, all JS and CSS assets are included as static files and checked in to Git. [TailwindCSS](https://tailwindcss.com/docs/installation#building-for-production) and [Iconify](https://iconify.design/docs/icon-bundles/) require an additional build step. To only require this at the time of development, the compiled assets are checked in to Git as well.

```bash
$ yarn
$ yarn build  # or: yarn watch
```

New icons can be added by editing the `icons` array in [scripts/bundle_icons.js](scripts/bundle_icons.js).

#### Precompression

As explained in [#284](https://github.com/muety/wakapi/issues/284), precompressed (using Brotli) versions of some of the assets are delivered to save additional bandwidth. This was inspired by Caddy's [`precompressed`](https://caddyserver.com/docs/caddyfile/directives/file_server) directive. [`gzipped.FileServer`](https://github.com/muety/wakapi/blob/07a367ce0a97c7738ba8e255e9c72df273fd43a3/main.go#L249) checks for every static file's `.br` or `.gz` equivalents and, if present, delivers those instead of the actual file, alongside `Content-Encoding: br`. Currently, compressed assets are simply checked in to Git. Later we might want to have this be part of a new build step.

To pre-compress files, run this:

```bash
# Install brotli first
$ sudo apt install brotli  # or: sudo dnf install brotli

# Watch, build and compress
$ yarn watch:compress

# Alternatively: build and compress only
$ yarn build:all:compress

# Alternatively: compress only
$ yarn compress
```

## ❔ FAQs

Since Wakapi heavily relies on the concepts provided by WakaTime, [their FAQs](https://wakatime.com/faq) largely apply to Wakapi as well. You might find answers there.

<details>
<summary><b>What data are sent to Wakapi?</b></summary>

<ul>
  <li>File names</li>
  <li>Project names</li>
  <li>Editor names</li>
  <li>Your computer's host name</li>
  <li>Timestamps for every action you take in your editor</li>
  <li>...</li>
</ul>

See the related [WakaTime FAQ section](https://wakatime.com/faq#data-collected) for details.

If you host Wakapi yourself, you have control over all your data. However, if you use our webservice and are concerned about privacy, you can also [exclude or obfuscate](https://wakatime.com/faq#exclude-paths) certain file- or project names.
</details>

<details>
<summary><b>What happens if I'm offline?</b></summary>

All data are cached locally on your machine and sent in batches once you're online again.
</details>

<details>
<summary><b>How did Wakapi come about?</b></summary>

Wakapi was started when I was a student, who wanted to track detailed statistics about my coding time. Although I'm a big fan of WakaTime I didn't want to pay <a href="https://wakatime.com/pricing">$9 a month</a> back then. Luckily, most parts of WakaTime are open source!
</details>

<details>
<summary><b>How does Wakapi compare to WakaTime?</b></summary>

Wakapi is a small subset of WakaTime and has a lot less features. Cool WakaTime features, that are missing Wakapi, include:

<ul>
  <li>Leaderboards</li>
  <li><a href="https://wakatime.com/share/embed">Embeddable Charts</a></li>
  <li>Personal Goals</li>
  <li>Team- / Organization Support</li>
  <li>Additional Integrations (with GitLab, etc.)</li>
  <li>Richer API</li>
</ul>

WakaTime is worth the price. However, if you only need basic statistics and like to keep sovereignty over your data, you might want to go with Wakapi.
</details>

<details>
<summary><b>How are durations calculated?</b></summary>

Inferring a measure for your coding time from heartbeats works a bit differently than in WakaTime. While WakaTime has <a href="https://wakatime.com/faq#timeout">timeout intervals</a>, Wakapi essentially just pads every heartbeat that occurs after a longer pause with 2 extra minutes.

Here is an example (circles are heartbeats):

```text
|---o---o--------------o---o---|
|   |10s|      3m      |10s|   |

```

It is unclear how to handle the three minutes in between. Did the developer do a 3-minute break, or were just no heartbeats being sent, e.g. because the developer was staring at the screen trying to find a solution, but not actually typing code?

<ul>
  <li><b>WakaTime</b> (with 5 min timeout): 3 min 20 sec
  <li><b>WakaTime</b> (with 2 min timeout): 20 sec
  <li><b>Wakapi:</b> 10 sec + 2 min + 10 sec = 2 min 20 sec</li>
</ul>

Wakapi adds a "padding" of two minutes before the third heartbeat. This is why total times will slightly vary between Wakapi and WakaTime.
</details>

## 👏 Support

Coding in open source is my passion and I would love to do it on a full-time basis and make a living from it one day. So if you like this project, please consider supporting it 🙂. You can donate either through [buying me a coffee](https://buymeacoff.ee/n1try) or becoming a GitHub sponsor. Every little donation is highly appreciated and boosts my motivation to keep improving Wakapi!

## 🙏 Thanks

I highly appreciate the efforts of **[@alanhamlett](https://github.com/alanhamlett)** and the WakaTime team and am thankful for their software being open source.

Moreover, thanks to **[Frachtwerk](https://frachtwerk.de)** for sponsoring server infrastructure for Wakapi.dev.

![](.github/assets/frachtwerk_logo.png)

## 📓 License

MIT @ [Ferdinand Mütsch](https://muetsch.io)
