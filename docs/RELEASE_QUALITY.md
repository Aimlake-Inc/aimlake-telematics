# Open-source release quality

**Status:** Checklist for public readiness — complete as the repo matures.

## Required artifacts

| Artifact | Location |
|---|---|
| Professional README | `README.md` |
| Architecture diagrams | `docs/` (add as built) |
| Quick-start | `docs/SELF_HOSTING.md` + README |
| API reference | OpenAPI + `docs/API.md` |
| SDK documentation | `sdk/*/README.md` |
| Contribution guidelines | `CONTRIBUTING.md` |
| Security disclosure | `SECURITY.md` |
| Code of conduct | `CODE_OF_CONDUCT.md` |
| Changelog | `CHANGELOG.md` |
| Issue templates | `.github/ISSUE_TEMPLATE/` |
| PR template | `.github/pull_request_template.md` |
| Roadmap | `ROADMAP.md` |
| License | `LICENSE` (Apache 2.0) |

## Release engineering

- Semantic versioning
- Automated releases (GitHub Actions)
- Software bill of materials (SBOM)
- Signed release artifacts where practical
- Automated tests in CI
- Clear licensing and dependency docs for SDKs, dashboard, server, and deploy scripts
