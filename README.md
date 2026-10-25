# KestrelSovereignAI agent identities

This repository hosts W3C [DID documents](https://www.w3.org/TR/did-core/) for agents in the KestrelSovereignAI fleet, served via GitHub Pages at:

- **`https://agents.kestrelsovereign.com/<agent-slug>/did.json`**

Each `did.json` is the public-key + verification-method document for one agent. The corresponding private keys are NOT in this repository — they live in encrypted at-rest storage on the agent's host.

## Why `did:web`

`did:web` is one of several DID methods Kestrel supports for agent identity. We use it for hybrid post-quantum agents because:

- The DID document natively lists multiple verification methods, so the same identity can carry both a classical (Ed25519) and post-quantum (ML-DSA-65) public key.
- A consumer fetches one URL, parses one JSON, and has everything needed to verify any signature from the agent.
- Identifier is permanent as long as the domain is. We chose `kestrelsovereign.com` (controlled by KestrelSovereignAI) rather than `*.github.io` so the identity is not tied to GitHub's infrastructure forever — only the current hosting is.

## MCP OAuth client identity

The local Kestrel MCP client identifies itself with this public Client ID
Metadata Document (CIMD):

- **`https://agents.kestrelsovereign.com/oauth/kestrel-local-client.json`**
- Redirect: **`http://127.0.0.1:44721/oauth/callback`**

The document declares a native public client using authorization code and
refresh-token grants, with `token_endpoint_auth_method=none`. It contains no
secret or agent key. Clients must use PKCE S256, bind and validate their
authorization state, and keep grants scoped to the agent, connection, issuer,
resource and authorized scopes. The public client ID is an application identity;
it grants no authority over any agent or vendor account.

The loopback callback is for a browser and Kestrel running on the same computer.
The client must own the declared listener before opening authorization and fail
clearly if that port is unavailable. A remotely hosted console needs its own
reviewed HTTPS callback and client metadata; it cannot use this local redirect.

Use this identity only after the authorization server advertises CIMD support.
Servers requiring a registered client need an explicitly configured registration;
do not silently register dynamically or borrow another product's client ID.
Publishing this document does not establish vendor acceptance, token refresh after
restart or permission to invoke tools. Those remain part of the three-vendor
spike and governed MCP client implementation in
[#3679](https://github.com/KestrelSovereignAI/kestrel-sovereign/issues/3679).

The client ID must remain stable. Moving its URL creates a different OAuth client,
and changing redirects or grant metadata can affect existing consent. The
document follows the
[OAuth Client ID Metadata Document draft](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-client-id-metadata-document-02).

## See also

- [`docs/architecture/security/SECURITY_OVERVIEW.md`](https://github.com/KestrelSovereignAI/kestrel-sovereign/blob/main/docs/architecture/security/SECURITY_OVERVIEW.md) — plain-language tour of Kestrel's cryptographic posture
- [`docs/architecture/security/SUCCESSION_RUNBOOK.md`](https://github.com/KestrelSovereignAI/kestrel-sovereign/blob/main/docs/architecture/security/SUCCESSION_RUNBOOK.md) — how a hybrid identity gets minted
