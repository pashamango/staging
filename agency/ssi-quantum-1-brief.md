# SSI quantum 1 — brief for codex@darwin

**Tier: ВП** (proposed — PM confirms). **Executor: codex@darwin only.**
**Status: brief only. Do NOT execute until Pavel taps this brief.**
**Date: 2026-11-13.** Staging release: `2026-11-13-r3`
(canary `AGENCY-TEST-SECRET-2026-11-13-r3-00e3ba10540260048ab66296f514dcdc` —
FAKE, proof-of-read only, opens nothing).

## Why this exists

The one blocker everywhere is onboarding a new employee/agent into
Forgejo/Plane: giving them the owner's admin key is a hole. The answer is
decentralised self-service SSI (charter: identity first, `did:key`,
self-issued; delegation, not impersonation; TTL ≤ 24h; every interaction
logged; sovereignty revocable). The Welcome Home gateway (branch
`feat/welcome-home-gateway`, commit
`d4a408f043414871b55c865e9bbef223f62fb597`) is HMAC-only — NOT SSI. The VC
issuer, the revocation registry and the DID adapter are still missing.
The SSI toolkit exists (MIN-106, `ssi.py`, Ed25519 + `did:key`, self-test
PASS; private keys in keychain as `ssi-<agent>`).

Quantum 1 is the minimal vertical slice: one agent, one DID, one scoped
capability, one revocation test, one audit log. Nothing more.

## The task (WHAT, not HOW)

1. **Self-issue a `did:key`.** Use the MIN-106 `ssi.py` toolkit (or its
   equivalent) on Darwin. The private key stays in the Darwin keychain
   (`ssi-codex-darwin` or equivalent). It is never printed, pasted, or
   committed. If the toolkit is unavailable, say so and stop — do not
   improvise a key by hand.
2. **Present the DID.** Present it at the staging entry:
   `https://staging.hackt.com/agency/<DID>/?ref=<invitation>` — open the
   entry bundle there (mandate template, ticket examples, current
   discussions, rights & obligations). Confirm the bundle renders for
   your DID.
3. **Receive a scoped VC for Forgejo READ of agency.git only.**
   Scope: `forgejo:agency:read`. Nothing else: no write, no admin, no
   Plane. The VC is issued through the Secure Vault flow — the issuer is
   whoever holds the registrar role (Muse/fleetd + Pavel tap on first
   enrolment). If no issuer exists yet, BUILD the minimal issuer as part
   of this quantum and say plainly that you built it — a stub presented
   as a real issuer is a cut corner (END != DONE).
4. **Use the VC to read agency.git.** Read-only: clone or fetch the
   `agency.git` mirror from staging and verify HEAD
   `8f6b4ea5b8fe5f758bdf56078a2a8ac8f925216c`. The read must go through the
   VC-gated path you built, not through a shared owner key and not
   through an unauthenticated public fetch.
5. **Revocation test.** Revoke the VC, then attempt the same read again.
   The read MUST fail after revocation. Record both outcomes (before:
   allowed; after: denied) with timestamps.
6. **Full audit log.** Every step — issuance, presentation, VC receipt,
   read, revocation, denied read — logged with timestamps, in the git
   graph (commit) and in the handoff.

## Entry test (proof of equal access)

Before anything else, answer the verification question from the staging
README: fetch `https://staging.hackt.com/agency/README.md` and quote
verbatim the test secret + the release number. Expected: the r3 canary
above + `2026-11-13-r3`. Quoting the r1 value, or "no test secret", is a
wrong answer — say so and stop.

## Hard constraints

- **No Plane write.** Pointer-only, and only through Muse/fleetd.
- **No admin key.** No shared owner key, ever. Your `did:key` is yours
  alone. Secrets live only in the Secure Vault / keychain, never in chat,
  prompts, files or logs.
- **TTL ≤ 24h.** The mandate expires (apoptosis); renewal is a new
  mandate. No standing capability.
- **EVERYBODY WRITES, NOBODY DELETES.** Write via PR from your own
  branch. No direct push to main, no force-push, no deletions.
- **Local main ≠ deployed version.** A conforming PR auto-accepts into
  local main. Deploy only by Pavel's tap.
- **Honesty over completeness.** If a piece cannot be built (no issuer,
  no registry), report it as missing with the exact blocker. A stub is
  fine if labelled a stub; a stub labelled real is a failure.

## Acceptance criteria

- [ ] `did:key` self-issued on Darwin; private key in keychain only.
- [ ] Entry bundle renders at the staging URL for that DID.
- [ ] VC received, scope exactly `forgejo:agency:read`, TTL ≤ 24h.
- [ ] Read of agency.git via the VC-gated path succeeds; HEAD verified.
- [ ] After revocation, the same read is denied. Both outcomes logged.
- [ ] Audit log complete (issuance → presentation → read → revocation →
      denied read), with timestamps.
- [ ] Entry-test answer quoted (canary + release).
- [ ] Handoff: repo + branch + commit + reproducible checks.

## Handoff

Reply in the Linear/Plane thread with: repo URL, branch, commit, the
VC scope, the revocation test outcome (allowed/denied with timestamps),
and the audit log. Then move the ticket to In Review with label
`Role: QA-Needed`. Done only by Pavel's tap.
