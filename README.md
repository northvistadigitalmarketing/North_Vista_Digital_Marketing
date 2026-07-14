# {{CUSTOMER_NAME}} — NVM Boost

Client delivery workspace. Created and maintained by Krista.

- **Client:** {{CUSTOMER_NAME}}
- **Slug:** {{CUSTOMER_SLUG}}
- **Domain:** {{DOMAIN}}
- **Active Lines:** {{ACTIVE_LINES}}

## What's where

| Folder | In plain terms |
|---|---|
| `docs` | **The live website** — landing pages published to the web. (GitHub requires the publishing folder be named `docs`; think of it as "the site.") |
| `campaigns` | Ad copy, campaign specs, and email campaigns |
| `reports` | Monthly performance reports and audits |
| `client-profile` | The living client profile — scope of work, brand voice, delivery history |

## Naming notes for non-developers

- `index.html` inside `docs` is the site's **homepage** — that filename is a web standard and can't change. Additional landing pages get plain-English names, e.g. `heel-pain-landing-page.html`.
- The full delivery history is under the **Commits** link on this page — every filing, stamped and dated.
- Any file ending in `.md` renders as a formatted document when you click it.

## Conventions (for Krista and anyone filing manually)

- Krista commits directly to `main`. No branches, no PRs.
- Commit message format: `[<Line Name>] <description> – YYYY-MM-DD`
- Multi-file deliverables land as one atomic commit.
- Placeholder tokens (`{{...}}`) are resolved by Krista at provisioning time.