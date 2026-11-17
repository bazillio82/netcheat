# Netcheat AWS CI/CD

Static website deployed using AWS CI/CD.

## Architecture

Git → GitHub → AWS CodePipeline → AWS CodeBuild → Manual Approval → Amazon S3

## Components

- GitHub repository: bazillio82/netcheat
- AWS Region: eu-central-1
- CodeConnection: github-netcheat
- CodeBuild project: netcheat-build
- CodePipeline: netcheat-pipeline
- S3 bucket: netcheat-oleksii-bondar-2026-862939497077

## Build validation

CodeBuild uses `buildspec.yml` to:

1. Install `htmlhint`
2. Validate `index.html` and `error.html`
3. Verify that `index.html` contains `Шпаргалка`

## Additional tasks

- Added `tracepath example.com` to the networking commands table
- Added validation with `grep -q "Шпаргалка" index.html`
- Added Manual Approval before production deployment

## Deployment flow

Push to `main` automatically triggers the pipeline:

Source → Build → ManualApproval → Deploy

The Deploy stage extracts the CodeBuild artifact into the S3 website bucket.