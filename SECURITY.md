# Security

## Reporting a security issue

Do not open a public issue. Use **Report a vulnerability** under this repository's **Security** tab, so only the
maintainers see the report. Describe what you found, how to reproduce it and what it affects. We reply as soon as we
can.

## What is in place

- Force push and deletion of `main` are blocked (branch ruleset «Main protection»).
- Pull requests run `dependency-review` (no dependencies with known vulnerabilities), `workflow-security` (zizmor) and
  CodeQL code scanning.
- Actions are pinned to commit SHAs and updated monthly by Dependabot, with a cooldown of 7 days (30 for major
  versions). Patch and minor updates of Actions are merged automatically when all required checks pass
  (`dependabot-automerge.yml`). Other updates are merged by hand.
- Workflows get no access by default (`permissions: {}`); each job asks only for what it needs.
- Secrets live in GitHub environments limited to `main`, never in the code. `.gitignore` blocks `.env` and key files.
- Secret scanning with push protection, Dependabot alerts and security updates are on.

## Specific to this repository

- Upptime commits its results to `main` with the workflow token (`GITHUB_TOKEN`). The Upptime jobs ask for Contents
  and Issues, nothing else.
- The jobs that commit keep that token in the checkout. The other jobs use `persist-credentials: false`.
- The status page is served by GitHub Pages, which cannot set security headers. The page is static, with no forms,
  cookies or third-party scripts.
