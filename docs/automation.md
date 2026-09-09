Repository automation is active; application and infrastructure implementation remains a scaffold.

- `.github/workflows/repository-checks.yml` runs on pull requests, main pushes, manual dispatch, and weekly. It scans all fetched Git history with Gitleaks, validates workflows with actionlint, and checks changed lines for whitespace errors.
- Tools are version-pinned and release archives are SHA-256 verified. Checkout is pinned to a full commit, credentials are not persisted, and workflow permissions are read-only. No repository or cloud secrets are required.
- Gitleaks runs as its open-source CLI; this setup does not use the separately licensed Gitleaks organization Action. Findings are redacted and fail the check. A scan failure must be investigated; do not suppress it to merge.
- GitHub repository settings enable Dependabot alerts and automated security fixes, and require full commit SHA pins for Actions. These settings were applied through the API.
- Dependabot checks GitHub Actions weekly with grouped updates. Package updates will be configured once real package manifests exist. Dependabot does not update the shell-installed Gitleaks/actionlint versions: review their releases monthly, update URLs and checksums together from official release metadata, and rerun CI.
- `.gitignore` excludes common local credentials and generated files. It cannot remove secrets already committed; rotate any exposed credential and follow up on history cleanup.
- Current organization: private repositories on GitHub Free. GitHub rejected branch protection as unavailable on this plan. A passing check is visible but does not prevent a direct push or merge. Use PR review by team convention until enforceable protection is available.
- Build/test, Checkov, cloud OIDC/deployment, and Sentry are pending real application/IaC configuration and provider decisions. These repository checks do not establish application correctness, infrastructure safety, or runtime monitoring.
- Hosted Actions consume the organization Actions allowance. This setup changes no subscription or billing budget.

When tools report an error, open the Actions run and inspect the failed step. For a secret finding, revoke/rotate it before fixing the file; do not paste the secret into an issue. For a workflow error, fix the reported YAML/expression and rerun the check.
