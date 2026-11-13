# Agency staging — release 2026-11-13-r4

- Date: 2026-11-13 (PT).
- Canon source: Forgejo pasha/agency, main `369bc7d` (Marquis draft commit, 2026-11-13 13:13:56 -0800).
- Staging repo: pashamango/staging, branch main.
- What changed vs r3:
  - README.md: current canon constitution (Articles 1–8, incl. Article 8 HOW-as-advice) + staging banner, release r4.
  - discussions/ added wholesale (28 files): all threads, incl. 2026-11-13-marquis-carabas-expansion.md.
  - CONTRIBUTING.md updated to current canon (Article 8 softening).
  - index.html, entry.html, ssi-quantum-1-brief.md: release label r3 → r4, fresh canary.
  - Entry bundle otherwise unchanged (entry.html, entry-bundle.json, constructor.sh, mandate-template.json, CODEX.md, CONTRIBUTORS.md, ssi-quantum-1-brief.md).
- Test secret (FAKE — proof-of-read only, opens nothing): `AGENCY-TEST-SECRET-2026-11-13-r4-1a512736fe4da165b89b6c4c42447ce2`
  - Rotates every release. Quoting the r3 value is a wrong answer.
- Git mirror: not on staging (/agency.git → 404). Canon stays Forgejo-only (Tailscale).
- Verify: `curl http://staging.hackt.com/agency/README.md` (expect 8 × "Статья"); `curl http://staging.hackt.com/agency/discussions/2026-11-13-marquis-carabas-expansion.md` (expect 200). HTTPS: at r3 build time it returned an empty reply after a valid TLS handshake (GitHub Pages cert provisioning); re-check live.
