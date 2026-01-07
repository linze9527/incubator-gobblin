# Copilot / AI agent instructions for Apache Gobblin

Purpose: Provide focused, actionable context so an AI coding agent can be immediately productive in this repository.

High-level architecture (big picture)
- Gobblin is a modular Java/Gradle ETL framework composed of many `gobblin-*` modules (examples: `gobblin-core`, `gobblin-runtime`, `gobblin-service`).
- Modules are separate Gradle subprojects (see top-level `settings.gradle` and each module's `build.gradle`).
- Runtime variants and deployment targets are separated into directories like `gobblin-yarn`, `gobblin-docker`, `gobblin-kubernetes`, `gobblin-oozie`, and `gobblin-hive-registration`.

Key developer workflows
- Build full project: use the wrapper: `./gradlew clean build` from repository root.
- Build or test a single module: `./gradlew :gobblin-core:build` or `./gradlew :gobblin-core:test`.
- Packaging/distribution logic lives under `gobblin-distribution` and `gobblin-distribution/*` flavor files.
- Runtime launcher scripts live in `bin/` (examples: `bin/gobblin`, `bin/gobblin-service.sh`, `bin/gobblin-standalone.sh`). Use them for local runs and to learn expected CLI flags and env vars.

Project-specific conventions and patterns
- Module naming: prefix `gobblin-`; each module is a Gradle subproject with its own `build.gradle`.
- Config and job locations: runtime configs and examples appear under `conf/`, `config/`, and `jobconf/` — look here for canonical property names and example job definitions.
- Tests and harnesses: see `gobblin-test`, `gobblin-test-harness`, and `gobblin-test-utils` for test utilities and patterns used across modules.
- Distribution flavors: `gobblin-distribution/gobblin-flavor-*.gradle` show how different packaging variants are constructed; copy pattern when adding a new distribution flavor.

Integration points & external dependencies
- Hadoop/YARN: repository targets Hadoop 2.x runtimes (see docs in `gobblin-docs` and top-level docs). Build and runtime are tied to Hadoop compatibility.
- Cloud & services: `gobblin-aws`, `gobblin-kubernetes`, `gobblin-docker`, `gobblin-hive-registration`, and `gobblin-oozie` indicate common integrations — changes that affect core APIs often need cross-module updates.

Where to look for examples and anchors
- Top-level build: `build.gradle`, `gradlew`, `settings.gradle`.
- Core logic: `gobblin-core/src` and `gobblin-runtime/src`.
- Service / runtime examples: `gobblin-service/`, `bin/` scripts, and `gobblin-distribution/`.
- Job/config examples: `jobconf/`, `conf/`, `config/`.
- Tests: `gobblin-test/`, `gobblin-test-harness/`.

Practical suggestions for editing & PRs
- Prefer changing or adding a single Gradle subproject for narrow features. Run `./gradlew :that-module:build` locally before broader builds.
- When modifying runtime behaviour, update the corresponding script under `bin/` or the distribution flavors to ensure packaging includes new artifacts.
- Keep changes backward compatible with Hadoop 2.x unless explicitly updating compatibility; mention compatibility changes clearly in PR descriptions.

Concrete examples (copy-paste)
- Build and test a module:
  `./gradlew :gobblin-core:clean :gobblin-core:build`
- Run unit tests for entire repo (may be slow):
  `./gradlew test`
- Run the standalone runner locally (reads configs in `conf/`):
  `bin/gobblin-standalone.sh --config-file conf/standalone.example.properties`

Limitations of these instructions
- This file documents only discoverable, repo-local patterns (build, layout, runtime scripts). It does not attempt to describe runtime behavior of every module or runtime cluster operations beyond what `bin/` and module directories show.

If anything here is unclear or you want additional examples (e.g., common refactors, hot-spot files, or a short checklist for PRs), tell me which area to expand.
