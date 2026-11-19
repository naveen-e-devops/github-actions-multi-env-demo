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

| Event / ref | DEV | STAGE | PROD |
| --- | --- | --- | --- |
| Push to default branch (`main`) | Automatic | Manual only | Automatic |
| Push to `feature/**` | Automatic | Manual only | Not available |
| Push a tag (for example, `v1.0.0`) | Not available | Not available | Automatic |
| Manual run on default or feature branch, select STAGE | No | Runs | No |

- Pushes to `main` and `feature/**` automatically start DEV.
- STAGE is opt-in through **Actions → Multi-Environment CI-CD Pipeline → Run workflow**, using the `run_stage` boolean input. It is available on the default branch and `feature/**` branches.
- PROD runs automatically when code is pushed to `main` or when any Git tag is pushed.
- PROD does not have a manual input.
- PROD is not run from feature branches.

## GitHub Environments

Create these environments under **Repository → Settings → Environments**:

- `dev`
- `stage`
- `prod`

For production, consider requiring reviewer approval and restricting deployment branches to `main` and the tag patterns you use for releases.

## GitHub Actions components demonstrated

- Workflow triggers: `push`, `workflow_dispatch`
- Manual boolean input for STAGE
- Workflow-level token permissions
- Multiple jobs and Ubuntu runners
- Steps and shell commands
- Job dependencies with `needs`
- Conditional execution with `if`
- Deployment environments with `environment`

## Try it

1. Push a commit to `main`: DEV and PROD run automatically.
2. Push a commit to `feature/demo`: DEV runs automatically.
3. Open the Actions tab and select **Multi-Environment CI-CD Pipeline → Run workflow**. Select a default or feature branch and enable `run_stage`.
4. Create and push a release tag to trigger PROD:

   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
