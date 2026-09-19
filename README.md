# AI Educademy Pro Courses

Private subscriber course content for [AI Educademy](https://aieducademy.org).

This repo contains Pro lessons for the paid learning product. The first lesson of every programme is free through the public `ai-courses` repo. The remaining lessons require Pro at £3.99/month, £29.99/year, or £49.99 lifetime.

## Programmes

| Programme | Track | Level | Estimated study time |
|-----------|-------|-------|----------------------|
| AI Branches | Understanding AI | 3 | 10 hours |
| AI Canopy | Understanding AI | 4 | 18 hours |
| AI Forest | Understanding AI | 5 | 24 hours |
| AI Chisel | Craft and Engineering | 2 | 14 hours |
| AI Craft | Craft and Engineering | 3 | 18 hours |
| AI Polish | Craft and Engineering | 4 | 10 hours |
| AI Masterpiece | Craft and Engineering | 5 | 30 hours |
| Behavioural Mastery | Career Ready | 2 | 8 hours |
| Technical Interviews | Career Ready | 3 | 16 hours |
| AI and ML Interviews | Career Ready | 4 | 14 hours |
| Offer and Beyond | Career Ready | 5 | 8 hours |

## Structure

```text
programs/
├── ai-branches/
│   ├── program.json
│   └── lessons/
└── ...
```

Each programme has a `program.json` manifest and MDX lessons. Content is consumed by `ai-platform` during builds and runtime content loading.

## Working with content

- Keep lesson frontmatter keys in English.
- Keep examples accurate and current.
- Do not publish paid lesson text into public repos, issues, or PR bodies.
- Link learners to [aieducademy.org](https://aieducademy.org), not preview domains.
- Keep commercial wording aligned with the current Pro plans.

## Related repos

| Repo | Purpose |
|------|---------|
| [`ai-platform`](https://github.com/ai-educademy/ai-platform) | Production Next.js app |
| [`ai-courses`](https://github.com/ai-educademy/ai-courses) | Public first lessons and blog content |
| [`ai-ui-library`](https://github.com/ai-educademy/ai-ui-library) | Shared React component library |

## Licence

© 2026 AI Educademy. All rights reserved. This content is proprietary and intended for the paid Pro product.
