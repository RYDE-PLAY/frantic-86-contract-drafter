# Contract Drafter Delivery Report

Frantic #86 is delivered as a published `contract-drafter` skill that turns an
explicit template, parties, and deal terms into a review draft. It records every
baseline departure and emits a separately gated `runx/send-as` proposal. The
skill does not approve legal terms or send the draft.

## Submitted Package

- Package: `ryde-play/contract-drafter@sha-8f10f3657a3c`
- Registry: https://runx.ai/x/ryde-play/contract-drafter@sha-8f10f3657a3c
- Source: https://github.com/RYDE-PLAY/runx/tree/a7ee625855088af9394900ac0e79b667cfe47b76/skills/contract-drafter
- PR: https://github.com/runxhq/runx/pull/334
- Raw X.yaml: https://raw.githubusercontent.com/RYDE-PLAY/runx/a7ee625855088af9394900ac0e79b667cfe47b76/skills/contract-drafter/X.yaml
- Raw SKILL.md: https://raw.githubusercontent.com/RYDE-PLAY/runx/a7ee625855088af9394900ac0e79b667cfe47b76/skills/contract-drafter/SKILL.md
- Registry digest: `sha256:18710b89ac00256fc41ca93788b1c75a1e19c9353d166ea5f71e836e980f3109`
- Profile digest: `sha256:a37fb2462c37f4a2aefae049f56121eee4950588e5b9e77d90e21d72a430c504`
- Dogfood receipt: `runx:receipt:sha256:4dc88c84fd87417183cb31873422178c9b9e227b953b76d8aacac7bb7f17a568`

## Contract Behavior

- Typed inputs are `template`, `parties`, and `terms`.
- Typed outputs include `draft_doc`, `deviations[]`, and `send_proposal`.
- Templates use explicit `[[path]]` placeholders restricted to scalar
  `parties.*` and `terms.*` paths.
- Required parties, terms, clauses, baseline mappings, placeholders, and
  downstream proposal fields are validated before a draft is created.
- The runner never creates a missing party, clause, or term and never falls
  back to a baseline value.
- Every deviation records its clause, term, baseline value, proposed change,
  and source path.
- The proposal names `runx/send-as` runner `plan`, but remains
  `approved:false`, `gated_not_sent`, with `provider_action:null`.
- The generated receipt uses a `review` act and binds the draft ref as its
  artifact effect; it does not claim legal approval or delivery.

## Harness And Install

- `runx-cli 0.6.14` was used throughout.
- The local signed harness passed 2/2 before publication.
- The hosted registry harness passed 2/2 after publication.
- A clean `runx add` of the exact immutable version succeeded.
- The clean installed-package harness also passed 2/2.
- `complete-template-produces-gated-draft` sealed with a draft, four
  deviations, and an unapproved proposal.
- `missing-required-term-refuses-without-proposal` refused because
  `terms.payment_terms is required`; it emitted no draft or proposal.
- `registry-read.json`, `clean-install.json`, `hosted-harness.json`, and
  `installed-harness.json` contain the machine-readable results.

## Post-Publish Dogfood

- The dogfood input is a public, synthetic, nonbinding services agreement. It
  is realistic enough to exercise every typed field without publishing private
  contract or personal data.
- The exact input is pinned at
  https://raw.githubusercontent.com/RYDE-PLAY/frantic-86-contract-drafter/17d2f3db257bc5f35fd1770bc80435257f4d6d1d/public/dogfood-input.json.
- The command invoked
  `ryde-play/contract-drafter@sha-8f10f3657a3c` through the remote registry and
  supplied a dedicated runtime receipt directory.
- Runtime stdout returned `status=sealed`, run id
  `run_draft_7bd5705a3d7a`, and receipt id
  `sha256:4dc88c84fd87417183cb31873422178c9b9e227b953b76d8aacac7bb7f17a568`.
- The same receipt id was found in the dedicated receipt store. Its canonical
  JSON exactly matches `dogfood-run.json.receipt`; no replacement receipt was
  minted.
- Hosted harness receipts are separately listed as
  `sha256:88c59b...97b97` and `sha256:d7f2e7...7fc65f`, so neither can be
  confused with the submitted dogfood receipt.
- Stock runx graph execution labels the graph-turn subject ref as
  `type=harness`. This receipt is still the post-publish `runx skill` receipt:
  it is returned by the sealed runtime call, has trusted registry provenance,
  matches run id `run_draft_7bd5705a3d7a`, was selected from the runtime store,
  and contains the skill's `review` act rather than an observation act.
- `runx verify` returned `valid=true`, `signature.status=valid`,
  `signature.mode=production`, and no findings.

## Draft Result

- Draft ref: `runx:contract-draft:c29a7681049d5b7e`.
- Confidentiality changed from `3 years` to `2 years`.
- Governing law changed from `California` to `Delaware`.
- Liability cap changed from `Fees paid in the prior 12 months` to
  `USD 80,000 aggregate`.
- Payment terms changed from `Net 30` to `Net 45`.
- `send_proposal.consumer.inputs` matches the official `runx/send-as` plan
  contract for objective, principal, audience, content reference, consent
  basis, and operator context.
- The proposal requires human approval and send-as preflight; no provider
  action or delivery occurred.

## New User Runbook

- Check the CLI: `runx --version` (this delivery used `runx-cli 0.6.14`).
- Inspect metadata: `runx registry read ryde-play/contract-drafter@sha-8f10f3657a3c --registry https://api.runx.ai --json`.
- Install: `runx add ryde-play/contract-drafter@sha-8f10f3657a3c --registry https://api.runx.ai --json`.
- Prepare JSON objects matching `template`, `parties`, and `terms`; the public
  `dogfood-input.json` is a complete non-private example.
- Run: `runx skill ryde-play/contract-drafter@sha-8f10f3657a3c --registry https://api.runx.ai --receipt-dir <receipt-dir> --json` with the three
  `--input-json` arguments shown in `evidence.json.commands.dogfood`.
- Inspect `draft_doc`, every item in `deviations`, and the proposal gate before
  taking any downstream action.
- Verify this delivery receipt with the public key in
  `verification-key.json` and
  `runx verify --receipt runx-receipt.json --json`.

PR-head `js-checks`, `demo-checks`, and `gitleaks` pass, and local
`pnpm verify:fast` passes. The two remaining red PR jobs are repository-wide
baseline failures: the official harness sweep fails nine existing skills and
does not include `contract-drafter`; Rust fails the existing
`bundled_voice_profile_matches_canonical_workspace_profile` test.
