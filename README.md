# GitHub Actions Multi-Environment Demo

A minimal GitHub Actions example with independent DEV, STAGE, and PROD job chains.

## Workflow

Workflow file: `.github/workflows/multi-env-pipeline.yml`

Each environment has four sequential jobs:

1. **CI** — prints a sample CI-started message.
2. **Test** — prints sample Checkov scan, Sonar CI scan, and unit-test messages.
3. **Build** — prints a sample build message.
4. **Deploy** — prints a sample deployment message.

Every step uses `echo` only. No real scanning, testing, building, or deployment is performed.

## Branch behavior

| Branch | DEV | STAGE | PROD |
| --- | --- | --- | --- |
| Default branch (`main`) | Yes | Yes | Yes |
| Feature branches (`feature/**`) | Yes | Yes | No |
| Other branches | No | No | No |

The workflow runs on pushes to `main` and `feature/**`, and can also be started manually with `workflow_dispatch`. For manual runs, the selected branch determines which environment jobs run.

## GitHub Environments

Create these environments under **Repository → Settings → Environments**:

- `dev`
- `stage`
- `prod`

For production, consider requiring reviewer approval and restricting deployment branches to `main`.

## GitHub Actions components demonstrated

- Workflow name and triggers: `push`, `workflow_dispatch`
- Workflow-level token permissions
- Multiple jobs and Ubuntu runners
- Steps and shell commands
- Job dependencies with `needs`
- Conditional execution with `if`
- Deployment environments with `environment`

## Try it

- Push a commit to `main`: DEV, STAGE, and PROD job chains run.
- Push a commit to a branch such as `feature/demo`: DEV and STAGE job chains run.
- Start a manual run from **Actions → Multi-Environment CI-CD Pipeline → Run workflow** and select `main` or a feature branch.
