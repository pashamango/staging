# DRAFT: Honest Quorum Intersection — research note (filed, not published)

Status: DRAFT. Not reviewed, not approved, not published. Nothing in this
file is a claim by Pavel or by the forum. Filed as a record only.

Provenance:

- Source: ChatGPT shared conversation "Research Scout — Frugal Relay":
  https://chatgpt.com/s/t_6aea75c60dc0819184b09d0022ed613b
- The shared copy shows a single ChatGPT response; no user prompt is
  visible in it.
- Text drafted by ChatGPT in conversation with Pavel. Pavel has not
  approved the text.
- Filed by: Muse (coordinator), Darwin-local commit, per Pavel's tap
  2026-11-03 ("Polozhit' chernovikom v forum").
- Full text: at the source link above (not copied here).
- Requested destination in the draft: discussions/science/crypto. That
  path DOES NOT EXIST in this repo as of 2026-11-03 (checked:
  discussions/ contains README.md, THREADS.md, navodyashchie_voprosy).
  No discussion was created, and no duplicate check was possible from
  the ChatGPT side.

Summary (coordinator's paraphrase, not the draft's wording):

- The note proposes a formally verifiable recovery architecture for
  digital trust: versioned signed recovery manifests, parent-state
  hashes, monotonic epochs, recovery authorization after signing-key
  compromise, reversible state transforms, formal safety verification,
  auditable decisions by human and autonomous participants.
- Mathematical content: for n participants, at most t Byzantine, quorum
  size q, any two quorums intersect in at least 2q-n members; requiring
  an honest member in every intersection gives 2q > n+t; tolerating t
  unavailable members gives q <= n-t; both hold when n > 3t.
- The note itself states these bounds belong to established Byzantine
  quorum theory and disclaims inventing quorum intersection; the
  proposed contribution is applying the bounds to verifiable recovery,
  autonomous AI participation, and formal verification of safety.
- Stated limits: candidate architecture only; no implementation,
  benchmark, security audit, or peer review; treat as an open research
  proposal, not an established system, product, or deployed protocol.
- References in the draft are placeholder lines, not full citations;
  they must be completed before any publication.

Review requested by the draft (paraphrased):

1. How can recovery authority remain accountable to the humans affected
   by its decisions?
2. What additional safeguards prevent coercion, replay, collusion, and
   malicious key rotation?
3. How should autonomous AI agents participate in a quorum without being
   mistaken for independent trust domains?
4. Can formal verification make recovery decisions understandable and
   contestable by ordinary users?
5. What existing Byzantine quorum, proactive recovery, and distributed
   key-management literature should this work explicitly build upon?

Checklist before any publication decision:

- [ ] Prior-work check against Byzantine quorum literature.
- [ ] Full citations replace the placeholder reference lines.
- [ ] Formal proof check.
- [ ] Decide placement: new discussions/ section? Which one, by whose tap?
- [ ] Independent analysis / counterexample search (the draft explicitly
      invites counterexamples).

Related: the draft names "BIP450 Reversible Slot Layer" as research
notation, explicitly not an approved Bitcoin Improvement Proposal.
