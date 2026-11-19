# GitHub Actions Multi-Environment Demo

A minimal GitHub Actions example with separate DEV, STAGE, and PROD job chains.

## Workflow

Workflow file: `.github/workflows/multi-env-pipeline.yml`

Each environment has four sequential jobs:

1. **CI** — sample initialization message.
2. **Test** — sample Checkov scan, Sonar CI scan, and unit-test messages.
3. **Build** — sample build message.
4. **Deploy** — sample deployment message.

Every step uses `echo` only. No real scanning, testing, building, or deployment is performed.

## Trigger behavior

| Event / selected branch | DEV | STAGE | PROD |
| --- | --- | --- | --- |
| Push to default branch (`main`) | Automatic | Manual only | Manual only |
| Push to `feature/**` | Automatic | Manual only | Not available |
| Manual run on `main`, select STAGE | No | Runs | No |
| Manual run on `main`, select PROD | No | No | Runs |
| Manual run on feature branch, select STAGE | No | Runs | No |

- Pushes to `main` and `feature/**` automatically start DEV.
- STAGE and PROD are opt-in through **Actions → Multi-Environment CI-CD Pipeline → Run workflow**.
- On the manual run form, select `run_stage` to run STAGE or `run_prod` to run PROD.
- PROD is restricted to `main`. STAGE is available on the default branch and `feature/**` branches.
- If you manually select both inputs on `main`, both STAGE and PROD run.

## GitHub Environments

Create these environments under **Repository → Settings → Environments**:

- `dev`
- `stage`
- `prod`

For production, consider requiring reviewer approval and restricting deployment branches to `main`.

## GitHub Actions components demonstrated

- Workflow triggers: `push`, `workflow_dispatch`
- Manual boolean inputs
- Workflow-level token permissions
- Multiple jobs and Ubuntu runners
- Steps and shell commands
- Job dependencies with `needs`
- Conditional execution with `if`
- Deployment environments with `environment`

## Try it

1. Push a commit to `main` or `feature/demo`: DEV runs automatically.
2. Open the Actions tab and select **Multi-Environment CI-CD Pipeline → Run workflow**.
3. Select the branch and enable `run_stage` to run STAGE.
4. Select `main` and enable `run_prod` to run PROD.
