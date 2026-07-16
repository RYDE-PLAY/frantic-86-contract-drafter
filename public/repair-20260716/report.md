# Contract Drafter Repair Report

Package: ryde-play/contract-drafter@sha-e442a388dfa5

Source commit: 9e92a5044d9a3611e60bb504599653963a5beb78

PR: https://github.com/runxhq/runx/pull/334

Receipt: runx:receipt:sha256:815aab326a8b59afbc1efaf9e92b612e5b5ec24ba8fad0d7a972072a5cc3854c

## What changed

- The runner now trusts only `template.source_ref` and fetches the template document at runtime.
- The dogfood input uses the public template URL https://cdn.jsdelivr.net/gh/RYDE-PLAY/frantic-86-contract-drafter@main/public/repair-20260716/template-source.json.
- The default graph executes `plan-send` in the same run and then `bind-send-as` binds the result to the draft.
- The final packet records `send_proposal.status=send_as_plan_executed`, `send_as_result.sealed_in_same_graph=true`, the draft ref, and the content digest.

## Verification

- Registry read: success.
- Clean install: installed.
- Installed harness: passed (complete-template-fetches-source-and-executes-send-as-plan, missing-required-term-refuses-without-proposal).
- Dogfood run: run_draft_1f28a06127e4, status sealed.
- runx verify: valid=true, signature_mode=production.

## Reviewer notes

This repair directly addresses the rejection notes: the template is not hand-fed, and the send-as planning result is consumed inside the sealed run. Provider delivery is still intentionally not performed; the output is an approval-gated send-as plan bound to the draft, not a provider send.
