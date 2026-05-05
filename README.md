# KestrelSovereignAI agent identities

This repository hosts W3C [DID documents](https://www.w3.org/TR/did-core/) for agents in the KestrelSovereignAI fleet, served via GitHub Pages at:

- **`https://agents.kestrelsovereign.com/<agent-slug>/did.json`**

Each `did.json` is the public-key + verification-method document for one agent. The corresponding private keys are NOT in this repository — they live in encrypted at-rest storage on the agent's host.

## Why `did:web`

`did:web` is one of several DID methods Kestrel supports for agent identity. We use it for hybrid post-quantum agents because:

- The DID document natively lists multiple verification methods, so the same identity can carry both a classical (Ed25519) and post-quantum (ML-DSA-65) public key.
- A consumer fetches one URL, parses one JSON, and has everything needed to verify any signature from the agent.
- Identifier is permanent as long as the domain is. We chose `kestrelsovereign.com` (controlled by KestrelSovereignAI) rather than `*.github.io` so the identity is not tied to GitHub's infrastructure forever — only the current hosting is.

## See also

- [`docs/architecture/security/SECURITY_OVERVIEW.md`](https://github.com/KestrelSovereignAI/kestrel-sovereign/blob/main/docs/architecture/security/SECURITY_OVERVIEW.md) — plain-language tour of Kestrel's cryptographic posture
- [`docs/architecture/security/SUCCESSION_RUNBOOK.md`](https://github.com/KestrelSovereignAI/kestrel-sovereign/blob/main/docs/architecture/security/SUCCESSION_RUNBOOK.md) — how a hybrid identity gets minted
