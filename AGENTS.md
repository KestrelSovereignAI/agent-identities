# Agent identity publication

See `/Volumes/data2/projects/AGENTS.md` for global standards.

This repository serves public DID documents and OAuth client metadata through
GitHub Pages at `https://agents.kestrelsovereign.com/`. `.nojekyll` keeps the
tracked JSON files available directly. Existing agent directories contain the
public verification documents; `oauth/` contains client metadata documents.

Keep private keys, client secrets, authorization codes and tokens out of this
repository. OAuth client identity does not replace an agent DID or authorize
access to an agent's credentials or tools.

Validate changed JSON and the relevant protocol metadata before opening a PR.
Review the whole worktree diff first. After publishing, verify HTTP 200, JSON
content type and exact bytes at the declared URL without following redirects.
