# GitHub publishing

Account: `alinvarat`, display name **Linwrath_Pulse**. Site: `alinvarat/alinvarat.github.io` (GitHub Pages from `main`).

## Before any push
1. Scan the files to publish: `grep -rIlP '[\x{0E00}-\x{0E7F}]'` (Thai), `grep -rInE '/home/<user>|<personal email>|<unreleased board names>|<private archive names>'`, plus token patterns (`sk-`, `ghp_`, `gho_`, `github_pat_`, `AKIA`). Remove backups (`.backups/`), host config (`.netlify/`, `.openai/`, `.env*`) and `node_modules`.
2. Commit with the GitHub noreply address (`<id>+alinvarat@users.noreply.github.com`); never the personal email.
3. Never publish someone else's project as the user's (check `author`/`LICENSE`), unreleased boards, or library source.

## Repo hygiene
- Old work: make it **private and archive** it. Archived repos are read-only: unarchive → change visibility → archive again.
- Removing a file from a public site: also rewrite history (orphan branch, single commit, force-push) and confirm the old URL returns 404.
- Creating many repos: GitHub blocks bursts ("created too many repositories, too quickly"). Create them in a background loop, about 45 s apart, retrying every 2–3 min.
- Each skill repo: English README with `# <name>`, description, install line `git clone https://github.com/alinvarat/skill-<name>.git ~/.claude/skills/<name>`, link to `agent-skills`; topics `claude-code`, `agent-skills`, `agent-skill`; homepage https://alinvarat.github.io.

## Profile
- `gh api -X PATCH /user` needs the `user` scope, which the token may lack; if so, edit bio and company in the logged-in browser at github.com/settings/profile.
- Do not change billing or deploy to third-party hosts without asking the user.

## After pushing
Poll the live URL until the new marker appears, then render-check it once more.
