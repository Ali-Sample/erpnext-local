# erpnext-local

Local vanilla ERPNext for the Ali-Sample sandbox, pinned to the versions RHOS runs.
Based on [frappe_docker](https://github.com/frappe/frappe_docker)'s single-file quick start.
**Placeholder data only.**

| | Version |
|---|---|
| ERPNext | v16.37.0 |
| Frappe | v16.36.0 expected (RHOS CI); checked against the image after first start |
| MariaDB / Redis | 11.8 / 7.4 |

All versions live in [`versions.env`](versions.env), pinned by image digest. Change them there only.

## Start (from WSL, not Windows)

Clone inside the Linux file system (e.g. `~/erpnext-local`), not under `/mnt/c`: it's much faster.
`.env` holds local-only random passwords and is git-ignored.

```bash
printf 'DB_ROOT_PASSWORD=%s\nADMIN_PASSWORD=%s\n' "$(openssl rand -hex 16)" "$(openssl rand -hex 8)" > .env
docker compose --env-file versions.env --env-file .env up -d
docker compose --env-file versions.env --env-file .env logs -f create-site   # first start: a few minutes
```

Open http://localhost:8080 and sign in as `Administrator` with `ADMIN_PASSWORD` from `.env`.

## Stop, reset

```bash
docker compose --env-file versions.env --env-file .env down        # stop, keep data
docker compose --env-file versions.env --env-file .env down -v     # stop and delete all data
```
