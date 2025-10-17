# URL Shortner

A small Ruby on Rails application that provides URL shortening and redirection.

This repository contains the Rails app used to create, list, and redirect shortened URLs. It also exposes a JSON API under `/api/v1` for programmatic usage.

Key points:

- Rails ~> 8.0.2
- PostgreSQL as the primary database (gem `pg`)
- Puma as the app server
- Propshaft for assets and Importmap for JavaScript

Table of contents
- Features
- Prerequisites
- Setup (development)
- Running (development and Docker)
- API examples
- Tests
- Notes

## Features

- Create shortened URLs
- Redirect to target URL via short code
- Basic web UI and a JSON API (versioned under `/api/v1`)

## Prerequisites

- Ruby (matching your system/chruby/rbenv/rvm setup) — this app targets Rails 8 so use Ruby 3.2+ (recommended). If you have a `.ruby-version` file in your environment, prefer that.
- PostgreSQL (local or container)
- Node is not required for importmap-based setup, but having a modern system with coreutils is helpful.

Gems of note (from `Gemfile`): `rails` (~> 8.0.2), `pg`, `puma`, `propshaft`, `importmap-rails`, `turbo-rails`, `stimulus-rails`.

## Setup (development)

1. Install Ruby and PostgreSQL, then install bundler:

	gem install bundler

2. Install gems:

	bundle install

3. Create and migrate the database (adjust `config/database.yml` if needed):

	rails db:create
	rails db:migrate

4. (Optional) Seed data:

	rails db:seed

5. Start the app:

	rails server

The app will be available at http://localhost:3000 by default.

## Running with Docker

This repository includes a `Dockerfile` and a `docker-entrypoint` script. To build and run locally:

1. Build the image:

	docker build -t url_shortner:latest .

2. Run (example using a local PostgreSQL container or external DB — adapt env vars as needed):

	docker run --rm -p 3000:3000 \
	  -e RAILS_ENV=production \
	  -e DATABASE_URL=postgresql://user:password@host:5432/dbname \
	  url_shortner:latest

If you use Docker Compose in your environment, wire the database service and set `DATABASE_URL` appropriately.

## API examples

Create a short URL (JSON):

Request

  POST /api/v1/urls
  Content-Type: application/json

Body example

  { "url": { "target": "https://example.com" } }

Response

  201 Created
  {
	 "id": 1,
	 "short_code": "abc123",
	 "target": "https://example.com",
	 "short_url": "http://your-host/abc123"
  }

Redirecting

Visit `GET /:short_code` in a browser or via curl to be redirected to the target URL.

## Tests

Run the test suite:

  rails test

System tests use Capybara and Selenium (see `Gemfile` test group). Ensure a suitable browser driver is available when running system tests.

## Notes and tips

- Check `config/database.yml` and `config/environments/*.rb` for environment-specific settings.
- For security scanning, the project includes `brakeman` in development dependencies.
- For local debugging, `pry` is included.

If you'd like, I can also:

- Add a short usage section with concrete curl examples that match the running host/port.
- Add a badge or CI instructions (GitHub Actions) to run tests automatically.

---

Updated README to include setup, run, Docker, API, and test instructions.
