# Contract Drafter Repair 2026-07-18

Package: ryde-play/contract-drafter@sha-49f4e7a4ea6c

Source commit: 2ac6911dbaa3cd0f3df0f45f01482a647e7a9b7e

PR: https://github.com/runxhq/runx/pull/334

## What changed

- Replaced the inert inline send-as agent-task/resume path with a sealed graph path: plan-send -> mock-provider-send -> bind-send.
- Added package-local graph/send-as executable projection so the published package carries the send-as planning contract it composes.
- Added deterministic mock provider delivery/readback and final binding checks for draft_ref and content digest.
- Removed the dogfood-visible no_send_performed/provider_delivery_performed=false posture from final evidence.
- Added external markdown template support and dogfooded against Obvious Playbook's public MSA template, not the delivery CDN.

## Verification

- runx version: runx-cli 0.6.14
- Local unit test: node skills/contract-drafter/test.mjs passed.
- Local harness: runx harness ./skills/contract-drafter passed 2/2.
- verify:fast passed.
- Published version: https://runx.ai/x/ryde-play/contract-drafter@sha-49f4e7a4ea6c.
- Registry read/install succeeded; clean installed package includes graph/send-as and mock-send.mjs.
- Installed package harness passed 2/2: complete-template-fetches-source-and-executes-mock-send, missing-required-term-refuses-without-proposal.
- Post-publish dogfood command used ryde-play/contract-drafter@sha-49f4e7a4ea6c with external template https://raw.githubusercontent.com/obvious/playbook/master/legal-templates/legal-templates/master-services-agreement.md.
- Dogfood sealed receipt: runx:receipt:sha256:3a12270bd32648da2a991f23c2782c37ce83ce279aae29882e5aee924bb3413e.
- runx verify: valid=true, signature_mode=production.

## Dogfood Result

- Draft ref: runx:contract-draft:0ea9c1adceae29dd.
- Content digest: sha256:3f80941161550c11c8f3a2629e396716e54a8d2aa92ab0d8567478e3a7cd6603.
- Template fetch digest: sha256:9bf235ed4f8630d52e58df8b868ed47e8ae9e10b65b53d54c3bb712c809ae794.
- Deviations: 7.
- Send proposal status: mock_provider_sent.
- Provider delivery performed: true.
- Readback verified: true.
- Mock delivery id: mock-delivery:702f0698cb2ab11a.
- Readback digest: sha256:fbd09eab2041ea45db6d0d921cc6ddf74aee6b0013aa3c42214be46e7d964bf2.

## Reviewer Reproduction

```bash
runx add ryde-play/contract-drafter@sha-49f4e7a4ea6c --registry https://api.runx.ai
runx registry read ryde-play/contract-drafter@sha-49f4e7a4ea6c --registry https://api.runx.ai --json
runx skill ryde-play/contract-drafter@sha-49f4e7a4ea6c --registry https://api.runx.ai --json \
  --input-json template='<see dogfood-input.json>' \
  --input-json parties='<see dogfood-input.json>' \
  --input-json terms='<see dogfood-input.json>'
runx verify sha256:3a12270bd32648da2a991f23c2782c37ce83ce279aae29882e5aee924bb3413e --receipt-dir <receipt-dir> --json
```
