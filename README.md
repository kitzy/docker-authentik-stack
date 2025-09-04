# docker-authentik-stack

This stack runs [Authentik](https://goauthentik.io) as the identity provider for the homelab.  
It includes:

- **Postgres** – database backend  
- **Redis** – cache/message broker  
- **Server** – Authentik web UI and API  
- **Worker** – background tasks

## Networks

- Each service is on the stack’s private `default` network for internal communication.  
- The `server` container also joins the shared `proxy` network so Nginx Proxy Manager can forward requests from `auth.kitzy.net`.  
- **Postgres** and **Redis** are intentionally *not* on `proxy` to keep them isolated.

## Volumes

- `authentik-db` – Postgres data  
- `authentik-redis` – Redis data  
- `authentik-media` – user-uploaded files (logos, exports, etc.)  
- `authentik-custom-templates` – any custom templates for flows or emails

## Environment

At minimum define:

- POSTGRES_PASSWORD=
- AUTHENTIK_SECRET_KEY=
- AUTHENTIK_HOST=
- AUTHENTIK_URL=
- AUTHENTIK_COOKIE_DOMAIN=

## Notes

- The ports: section is commented out; all external access should flow through NPM via the `proxy` network.
- Back up nightly with pg_dump against the authentik Postgres database.
