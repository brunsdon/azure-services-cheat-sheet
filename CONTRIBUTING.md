# Contributing

Thanks for helping improve the Azure Services Cheat Sheet.

This repository is intended to be practical, readable, and useful in real architecture and delivery
conversations. Contributions should make Azure service choices easier, not just add more content.

## Contribution Principles

- Keep guidance practical and field-tested.
- Prefer clear trade-offs over generic service descriptions.
- Explain when not to use a service.
- Include security, cost, scaling, and operations notes where they matter.
- Use tables for service comparisons and decision points.
- Use Mermaid diagrams for architecture flows.
- Link to official Microsoft documentation when a link adds value.
- Avoid marketing language and vendor-neutral filler.

## Good Contributions

- Real-world gotchas.
- Service comparison tables.
- Production checklists.
- Small Azure CLI, Bicep, PowerShell, .NET, GitHub Actions, or Azure DevOps snippets.
- Architecture patterns with clear trade-offs.
- Dataverse / Dynamics 365 integration lessons.
- Corrections when Azure product behavior or names change.

## Markdown Guidelines

- Use one `#` heading per page.
- Add a blank line after every heading.
- Keep heading levels ordered.
- Put each bullet item on its own line.
- Keep tables to one row per line.
- Use fenced code blocks with language identifiers.
- Use `mermaid` fenced blocks for diagrams.
- Prefer relative links for files in this repository.
- Keep sections short enough to scan on GitHub.

## Architecture Guidance

When adding architecture content, try to answer:

- What problem does this solve?
- When should I use it?
- When should I avoid it?
- What breaks in production?
- How is it secured?
- How is it monitored?
- What cost or scaling trade-off matters?

## Pull Request Checklist

- Markdown renders cleanly in GitHub.
- Internal links work.
- Mermaid diagrams are valid.
- Code blocks have language identifiers.
- Examples avoid real secrets, tenant IDs, or customer names.
- New guidance is useful for developers, architects, technical leads, or integration specialists.
- Any official documentation links point to Microsoft Learn or relevant Microsoft-owned
  documentation.

## Tone

Write like a senior engineer helping another engineer make a decision:

- Concise.
- Specific.
- Practical.
- Honest about trade-offs.
- Focused on production reality.
