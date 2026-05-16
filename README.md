# DataVis_Release
Release builds for my WIP Data Visualisation app. Go to:
https://github.com/Burseys/DataVis_Release/releases

## Feedback and bug reports

Use the issue forms below so reports are labeled and structured correctly:

- Report a bug: https://github.com/Burseys/DataVis_Release/issues/new?template=bug_report.yml
- Share release feedback: https://github.com/Burseys/DataVis_Release/issues/new?template=feedback.yml
- Request a feature: https://github.com/Burseys/DataVis_Release/issues/new?template=feature_request.yml

## App URL prefill fields

The YML forms support prefill via query parameters. Use these field IDs:

- release_version
- os (bug report form)
- gpu (bug report form)

Example URLs your app can open:

- Bug report:
	https://github.com/Burseys/DataVis_Release/issues/new?template=bug_report.yml&release_version=v0.8.2&os=Windows%2011&gpu=NVIDIA%20RTX%204070
- Feedback:
	https://github.com/Burseys/DataVis_Release/issues/new?template=feedback.yml&release_version=v0.8.2
- Feature request:
	https://github.com/Burseys/DataVis_Release/issues/new?template=feature_request.yml&release_version=v0.8.2

## Issue character limits

This repo includes a GitHub Action that checks issue body length and selected section lengths:

- Workflow: .github/workflows/issue-length-guard.yml
- Trigger: issue opened, edited, or reopened
- Behavior: comments with violations and applies the label `needs-trim`

You can tune limits in the workflow env values:

- `MAX_TOTAL_CHARS`
- `HEADING_LIMITS_JSON`
- `OVER_LIMIT_LABEL`
