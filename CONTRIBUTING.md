# Contributing to OpenWallet Foundation Helm Charts

- Create new charts in the `charts/` directory using a unique, meaningful name matching the OWF project the chart is referencing.
- Bump the chart version in `Chart.yaml` for any changes.
- Ensure both the chart `README.md` and `CHANGELOG.md` have been updated to reflect the changes.
  - `@bitnami/readme-generator-for-helm` is used to ensure chart documentation is consistent.
  - `conventional-changelog-cli` is used to generate changelogs. 
- All charts are linted and tested automatically on PRs.
- On merge to `main`, changed charts are published to GitHub Pages.
