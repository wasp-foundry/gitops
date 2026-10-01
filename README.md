# gitops

Desired state of every application in the `wasp-foundry` org, deployed by the `foundry-apps` ApplicationSet of the wasp-idp cluster-zero.

- `apps/<app>/base/` — Kustomize base (Deployment, Service). Image name `app`, no tag.
- `apps/<app>/overlays/development/` and `overlays/production/` — the image tag per environment. One ArgoCD `Application` per app and environment (`<app>-development`, `<app>-production`).
- New apps arrive by pull request from Backstage (or `scripts/foundry/seed-bookinfo`), with the first commit's tag in both overlays.
- Each app's CI bumps `overlays/development` with a direct commit to `main`. Production changes only by pull request: run the `promote` workflow (`gh workflow run promote.yaml --repo wasp-foundry/gitops -f app=<app>`) and merge the PR it opens.
