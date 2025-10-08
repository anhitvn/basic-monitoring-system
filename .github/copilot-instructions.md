# Copilot instructions for basic-monitoring-system

Purpose
- Help AI coding agents make small, safe, high-value changes to this repository: fixes to YAML/compose files, updates to container images and versions, small docs fixes, and low-risk wiring (volumes/ports).

Quick architecture (what to know)
- This is a small Docker Compose-style monitoring stack expressed across YAML files in the repo.
- Key services and files:
  - `grafana-system-base.yml` — defines `grafana`, `prometheus`, and `node-exporter` services; ports are remapped (Grafana 3000 → host 9010; Prometheus 9090 → host 9011; node-exporter → host 9012).
  - `kuma.yml` — uptime-kuma service (ports 3001) and persistent data at `./data`.
  - `README.md` — quick list of images and last update dates.

Primary goals for changes
- Keep container images and ports consistent across files. Avoid breaking existing port mappings unless explicitly requested.
- Prefer non-destructive edits: add comments, improve documentation, fix YAML indentation, and update image tags only when provided with explicit new versions.

Project-specific conventions
- Service ports in these YAML files are remapped to high host ports (e.g., Grafana uses `9010:3000`). Preserve the host-side port numbers.
- Volumes are currently commented out in `grafana-system-base.yml`; when enabling volumes, match paths shown in the file (e.g., `./grafana_data:/var/lib/grafana`).
- Image tags include specific build metadata (e.g., `grafana/grafana:12.3.0-18329792253`) — do not replace with `latest`.

Integration points & external deps
- Runtime expects Docker Engine / Docker Compose. Changes that add new services should include port mappings and optional volumes.
- Prometheus requires a `prometheus.yml` config file when enabled via the command flag in `grafana-system-base.yml`; do not remove the `--config.file` argument unless replacing it with a validated alternate.

Debug & developer workflows
- No build scripts or CI present. To run locally the developer uses Docker Compose-like commands against these YAML files (e.g., `docker compose -f grafana-system-base.yml -f kuma.yml up -d`).
- For quick verification, suggest using `docker ps` to confirm containers and `docker logs <container>` to inspect startup errors.

Patterns and examples to follow
- When adding or editing services, follow existing format: `services:` top-level, `image`, `container_name`, `restart`, (optional) `volumes`, `ports`, `networks`, and `depends_on`.
- Example: follow the `prometheus` block's use of `command:` as a YAML array rather than a single string.

When to ask the user
- If a change affects persistent data (enabling or changing volumes), ask before modifying data paths.
- If upgrading an image beyond a simple tag bump (e.g., architecture change, major version), ask for compatibility confirmation.

Safety rules (hard constraints)
- Never change host-side port numbers (left value in `host:container`) without user confirmation.
- Don't add secrets or plaintext credentials to repo files.
- Avoid adding or enabling services that require external credentials (e.g., cloud providers) without explicit instructions.

Files to reference when editing
- `grafana-system-base.yml` — primary compose file for Grafana + Prometheus + node-exporter
- `kuma.yml` — uptime-kuma service
- `README.md` — image versions and update notes

If you make edits
- Keep diffs minimal. Prefer comments explaining why the change was made.
- Run a quick smoke-check recommendation in the commit message: `docker compose -f grafana-system-base.yml -f kuma.yml up -d && docker ps --filter "name=grafana|prometheus|node-exporter|uptime-kuma"`.

Questions for the repo owner
- Do you prefer enabling persistent volumes by default, or keeping them commented for ephemeral runs?
- Are there additional Compose files or runtime scripts not checked in here?

If something is ambiguous, propose a small change and request review rather than committing large refactors.
