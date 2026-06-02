---
name: community-health-files
description: Use when the user asks about setting up a new open-source project, creating README/CONTRIBUTING/LICENSE/CODE_OF_CONDUCT/SECURITY/CITATION.cff files, adding GitHub issue or PR templates, or improving repository community standards.
metadata:
  references:
    - references/license-guide.md
    - references/citation-format.md
    - references/github-templates.md
    - references/setup-workflow.md
  assets:
    - assets/readme-template.md
    - assets/contributing-template.md
    - assets/code-of-conduct-template.md
    - assets/security-template.md
    - assets/citation-template.cff
---

# Community Health Files

## Primary Actions

- Create baseline community files for open-source repositories.
- Generate/adjust contribution, conduct, security, and support policies.
- Provide CITATION.cff metadata for research software citation.
- Create issue/PR templates for consistent triage and review.

## Core File Set

- Required: `README.md`, `LICENSE`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`
- Recommended: `SECURITY.md`, `SUPPORT.md`, `CITATION.cff`, issue/PR templates

## Setup and Validation Workflow

1. Create baseline files and template stubs.
2. Fill project-specific metadata (name, maintainers, contacts, citation fields).
3. Validate `CITATION.cff` with `cffconvert --validate`.
4. Verify issue/PR templates render correctly in GitHub UI.
5. Check repository Community Standards page and fill gaps.

## Templates

- [assets/readme-template.md](assets/readme-template.md)
- [assets/contributing-template.md](assets/contributing-template.md)
- [assets/code-of-conduct-template.md](assets/code-of-conduct-template.md)
- [assets/security-template.md](assets/security-template.md)
- [assets/citation-template.cff](assets/citation-template.cff)

## Deep References

- License selection and compatibility:
  [references/license-guide.md](references/license-guide.md)
- CITATION.cff fields and academic citation patterns:
  [references/citation-format.md](references/citation-format.md)
- GitHub issue/PR template details:
  [references/github-templates.md](references/github-templates.md)
- Setup sequence and validation checkpoints:
  [references/setup-workflow.md](references/setup-workflow.md)
