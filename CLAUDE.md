# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build & Run Commands

- **Build:** `./gradlew clean assemble`
- **Run tests:** `./gradlew test`
- **Run locally:** `java -jar -Dspring.profiles.active=<profile> build/libs/spring-music-1.0.jar`
  - Profiles: `mysql`, `postgres`, `mongodb`, `redis` (no profile = in-memory H2)
  - Only one profile may be active at a time
- **Deploy to Cloud Foundry:** `cf push` (uses `manifest.yml`)

## Architecture

This is a Spring Boot 2.4.0 application that stores music album data, demonstrating multiple persistence backends behind a single REST API.

### Key Design Pattern: Profile-based Database Selection

`SpringApplicationContextInitializer` detects which database to use via Spring profiles. It auto-excludes irrelevant auto-configurations (e.g., disables Mongo/Redis when using JPA). On Cloud Foundry, it reads bound service tags to activate the correct profile automatically.

### Persistence Layer

All repositories implement Spring Data's `CrudRepository<Album, String>`. The controller and populator depend only on `CrudRepository`, not concrete implementations:

- **JPA** (default/mysql/postgres/sqlserver/oracle) — `JpaAlbumRepository`
- **MongoDB** — `MongoAlbumRepository`
- **Redis** — `RedisAlbumRepository` (custom implementation using `RedisTemplate`)

`AlbumRepositoryPopulator` seeds data from `src/main/resources/albums.json` on first startup when the repository is empty.

### REST API

- `GET /albums` — list all
- `PUT /albums` — create
- `POST /albums` — update
- `GET /albums/{id}` — get by ID
- `DELETE /albums/{id}` — delete by ID
- `GET /appinfo` — application environment info
- `GET /errors` — Spring Boot error endpoint

### Frontend

AngularJS 1.2 SPA served as static resources from `src/main/resources/static/`. Uses Bootstrap 3 via webjars.

## Code Style

- Only add comments to complex logic. Do not comment straightforward or self-explanatory code.

## Tech Stack

- Java 8+, Gradle 6.7, Spring Boot 2.4.0
- Java CFEnv 2.1.2 for Cloud Foundry service binding detection
- JUnit 4 + SpringRunner for tests
