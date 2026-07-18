# card-shuffle

Demo monorepo for the Mafia card-shuffle game, wired against a ReARM CE
instance for the Black Hat 2026 demo.

## Layout

| Path           | Source                                                           | What it is                                |
|----------------|------------------------------------------------------------------|-------------------------------------------|
| `mafia-vue/`     | [taleodor/mafia-vue](https://github.com/taleodor/mafia-vue)      | Vue 3 frontend (Dockerfile + Nginx)       |
| `mafia-express/` | [taleodor/mafia-express](https://github.com/taleodor/mafia-express) | Node/Express backend (Dockerfile)         |
| `mafia-helm/`    | [relizaio/helm-charts](https://github.com/relizaio/helm-charts) `mafia/` | Helm chart wiring the two services + redis |

## CI

`.github/workflows/build.yml` runs on every push and submits build
metadata + SBOMs for all four components to the controlling ReARM CE
instance at `https://rearmce.rearmhq.com`. Container images are pushed
to `registry.relizahub.com/d7742f63-c1ef-4ab1-910b-722419180bda-public`
and the helm chart is OCI-pushed to the same namespace. `create_component`
is on so the components self-provision on first build.

Action pins: `relizaio/rearm-docker-action` 1.13.4,
`relizaio/rearm-helm-action` 1.10.2, `relizaio/rearm-actions` 1.7.0
(all SHA-pinned in the workflow).

## Secrets used

- `DOCKER_LOGIN`, `DOCKER_TOKEN` — registry auth (provisioned on the repo)
- `REARM_API_ID`, `REARM_API_KEY` — FREEFORM key for the ReARM org
