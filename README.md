# bordersys/actions

Composite actions shared across our repos — only the generic ones any repo
can use. App-specific actions stay with their apps (diorama's `sim`/`finish`,
conductor's `dokploy-deploy`).

```yaml
- uses: bordersys/actions/mint-release-tag@main
- uses: bordersys/actions/require-green-ci@main
- uses: bordersys/actions/await-workflow@main
```

## mint-release-tag

Tag the current commit with a `v*` version, auto-bumped (patch/minor/major)
from the latest remote tag unless an explicit version is given. `bump: auto`
infers the size from the head commit instead: conventional-commit breaking
markers (`feat!:`, `fix(scope)!:`, `BREAKING CHANGE:`) mint a minor — the
breaking lane on 0.x — and anything else a patch. Needs a
checkout with `fetch-tags: true` and a workflow with `contents: write`.
Pushes made with the workflow token never re-trigger workflows, so the run
that mints carries on — the conductor release pattern.

## require-green-ci

Find the CI run for the current commit and fail unless it finished green.
The gate that keeps "release" meaning "tested". Inputs: `token` (required),
`workflow` (default `ci.yml`), `commit` (default the current SHA),
`timeout-minutes` (default 30).

## await-workflow

Wait for the latest run of some workflow on a branch or tag to complete
successfully — for cross-workflow ordering, like waiting for a spec publish
before building the thing that consumes it. Inputs: `workflow` and `ref`
(required), `token` (required), `timeout-minutes` (default 20).

## Private-repo note

If this repo is private, other repos in the org can only use its actions
after allowing it: Settings → Actions → General → Access → "Accessible from
repositories in the organization".
