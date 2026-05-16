# Feedback & Bug-Report GitHub Forms — Setup Plan

## Overview

The `FeedbackUI` in-app panel opens GitHub Issue submission pages in the user's
default browser when the user clicks **Send Feedback** or **Report a Bug** from the
`Help` menu.

The URLs defined in `Engine/Core/ExternalLinks/KnownExternalLinks.cs` assume two
GitHub Issue _templates_ exist in the repository. This document describes what needs
to be done to create those templates so the pre-filled URLs work correctly.

---

## Current State

| URL constant | Target | Status |
|---|---|---|
| `KnownExternalLinks.SendFeedbackUrl` | `…/issues/new?labels=feedback&template=feedback.md` | ⚠️ Template not yet created |
| `KnownExternalLinks.ReportBugUrl` | `…/issues/new?labels=bug&template=bug_report.md` | ⚠️ Template not yet created |
| `KnownExternalLinks.NewIssueFallbackUrl` | `…/issues/new` | ✅ Always works (no template) |

If a `template=` query parameter names a file that does not exist, GitHub silently
falls back to the plain new-issue form. So the current URLs are **safe to ship** —
they will degrade gracefully until the templates are in place.

---

## Steps to Complete

### 1. Make the repository public (or use a public-facing issue tracker)

GitHub Issue templates only matter when external contributors can reach them. If the
repository is currently private, you can either:

- Make it public on GitHub Settings → General → Danger Zone.
- Or keep it private and accept that users submit feedback through the plain issue
  form (the fallback URL always works for collaborators).

### 2. Create `.github/ISSUE_TEMPLATE/` directory

```
.github/
  ISSUE_TEMPLATE/
    bug_report.md
    feedback.md
    config.yml          ← optional: disables blank issues, adds contact links
```

### 3. `bug_report.md` template

```markdown
---
name: "Bug Report"
about: "Report something that isn't working correctly"
labels: bug
assignees: ''
---

## Describe the bug
<!-- A clear and concise description of what the bug is. -->

## To reproduce
Steps to reproduce the behavior:
1. ...
2. ...

## Expected behavior
<!-- What you expected to happen. -->

## Screenshots / logs
<!-- If applicable, attach screenshots or paste relevant log output. -->

## Environment
- OS: [e.g. Windows 11, macOS 14, Ubuntu 24.04]
- DataVis version: [check Help → About or the release tag]
- GPU: [e.g. NVIDIA RTX 4070]

## Additional context
<!-- Any other context about the problem here. -->
```

### 4. `feedback.md` template

```markdown
---
name: "Feedback / Feature Request"
about: "Share an idea or suggestion for DataVis"
labels: feedback
assignees: ''
---

## Summary
<!-- A concise description of your feedback or feature request. -->

## Motivation
<!-- Why would this be useful? What problem does it solve? -->

## Proposed solution (optional)
<!-- If you have ideas about how to implement this, describe them here. -->

## Alternatives considered (optional)
<!-- Other approaches you've thought of. -->

## Additional context
<!-- Screenshots, mockups, links, or anything else that helps. -->
```

### 5. Optional: `config.yml` to add contact links

```yaml
blank_issues_enabled: false
contact_links:
  - name: DataVis Releases
    url: https://github.com/Burseys/DataVis/releases
    about: See the latest releases and changelogs
```

### 6. Verify URLs after templates are committed

After committing the templates, open a browser and test:

- `https://github.com/Burseys/DataVis/issues/new?labels=feedback&template=feedback.md`
- `https://github.com/Burseys/DataVis/issues/new?labels=bug&template=bug_report.md`

Each URL should load the corresponding pre-filled issue form.

---

## Updating `KnownExternalLinks.cs` If URLs Change

All external URLs live in one place:

```
Engine/Core/ExternalLinks/KnownExternalLinks.cs
```

Edit the constants there if:

- The repository is renamed or moved.
- Templates are renamed.
- A dedicated feedback platform (e.g. Canny, UserVoice) replaces GitHub Issues.

---

## Spam Guardrails Already Implemented

The `ExternalLinkService` in `Engine/Core/ExternalLinks/ExternalLinkService.cs` already
enforces:

| Guardrail | Value |
|---|---|
| Per-URL cooldown | 10 seconds between two opens of the same URL |
| Global burst cap | Max 5 URLs opened within any 60-second window |
| Scheme whitelist | Only `https://` and `http://` URLs are allowed |

These limits prevent both accidental double-clicks and deliberate automation.
