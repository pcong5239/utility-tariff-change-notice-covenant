# Utility Tariff Change Notice Covenant

An on-chain, consensus-backed trace for whether a public utility customer notice preserves the consequential fields of a regulator-approved tariff revision.

## Corrected R5 deployment

This is the exact corrected revision executed after local and PRE-DEPLOY gates. The complete scenario evidence is in [`verification/e2e-matrix.md`](verification/e2e-matrix.md), with machine-readable bindings in [`verification/manifest.json`](verification/manifest.json).

- Revision: `UTNC-PREDEPLOY-20260923-R5`
- Contract SHA-256: `E5C63156299251C5FE46E554FC2EC94B47951D68C0B100E3D9B0DCA6DA6E897A`
- Network: GenLayer Studio Dev, chain ID `61997`
- Contract: [`0x41CaD4d9BbC0Efe83cf06FE30eBa84b3ceF6Ab42`](https://explorer-studio-dev.genlayer.com/address/0x41CaD4d9BbC0Efe83cf06FE30eBa84b3ceF6Ab42)
- Explicit owner actor7: `0x8581C4A532DD3f9B163b12809b1bd089f367147f`
- Explicit non-owner actor8: `0x91cb572164c865cbf323cf866fda6e01e35c02d0`
- Deployment: [`0xb0850b4c8f8a90ba8e5fa8151100bc3de34911f1934a4aa98813679505314e40`](https://explorer-studio-dev.genlayer.com/tx/0xb0850b4c8f8a90ba8e5fa8151100bc3de34911f1934a4aa98813679505314e40), `FINALIZED / SUCCESS / MAJORITY_AGREE`
- E2E: `PASS` for E2E-01 through E2E-18 and E2E-20; E2E-19 is explicitly `NOT APPLICABLE` because the safe validator-split preflight returned `-32603 Internal error` before broadcast, with unchanged record state.

## The rejected baseline and correction

The rejected public revision is commit [`261663a1605af2099e0341fa4a5ebf1b41d90a65`](https://github.com/pcong5239/utility-tariff-change-notice-covenant/commit/261663a1605af2099e0341fa4a5ebf1b41d90a65). The rejection identified a griefing path: an unauthenticated first assessment could supply publication day `9999131`, consume the first assessment and prevent the owner from satisfying a strictly-newer correction-date rule.

R5 preserves the same trust problem, mechanism and scope while removing the unsafe caller-controlled date. Assessment methods accept only the notice manifest; correction ordering is bound to the accepted manifest and explicit `prior_revision`, and owner authorization is checked before nondeterministic work. The first assessment remains permissionless.

## Problem and mechanism

Tariff sheets and customer notices may use different public wording while referring to the same change. A single backend should not be the sole authority for a consequential public trace. The contract lets validators independently retrieve hash-bound public sources, extract a fixed decision tuple and reach consensus before the trace changes.

1. The owner registers one tariff revision, its public source identity, allowed domains and required criterion mask.
2. The owner seals the tariff identity; utility, jurisdiction, tariff, service class and criteria become immutable.
3. A permissionless assessment retrieves the tariff and notice sources and reaches consensus on the structured decision tuple.
4. Only accepted consensus mutates state. The aggregate result is derived from disjoint criterion masks: `TRACEABLE`, `MISMATCH`, `PARTIAL`, `INSUFFICIENT_PUBLIC_EVIDENCE` or `UNRESOLVED`.
5. Owner-authorized corrections use an explicit prior revision and retain readable history; accepted manifest replay is idempotent.

## Public API and invariants

Writes: `register_tariff`, `seal_tariff_revision`, `assess_customer_notice`, `reassess_corrected_notice`.

Views: `read_notice_trace`, `read_change_signal`, `read_criterion_masks`, `read_revision`.

`assess_customer_notice` takes `(record_id, notice_url, notice_sha256)`. `reassess_corrected_notice` takes `(record_id, prior_revision, notice_url, notice_sha256)`; neither accepts a caller publication date.

The contract validates IDs, URLs, domains, SHA-256 values, masks, dates, enums and bounded text before external retrieval. Matched, mismatch and missing masks are pairwise disjoint and cover the required mask. Every consequential field is consensus-bound. Failed retrieval, hash verification, parsing, malformed model output, VM/LLM errors and validator disagreement fail closed and preserve the prior accepted state. Protocol-level undetermined execution is not represented as a fabricated contract result.

## Verification

The Direct Mode suite passed `130` tests with the exact pins in [`requirements.txt`](requirements.txt); `compileall` and `pip check` passed. The pinned Windows Direct Mode loader has a known `PermissionError: [WinError 32]` before contract import, so the passing test command is run through WSL2/uv:

```bash
uv run --offline --with-requirements requirements.txt python -m pytest tests/test_utility_tariff_change_notice_covenant.py -q
uv run --offline --with-requirements requirements.txt python -m compileall -q contracts tests
```

The pinned `genvm-linter 0.11.1rc2` passed lint and semantic validation under `GENVM_VERSION=v0.6.0-rc5`; the deployed schema has 8 methods (4 view, 4 write). Deployed source readback matched the candidate source line-for-line.

The E2E matrix records every write operation ID, transaction hash, Explorer URL, finality, semantic outcome, consensus result and authoritative readback. Fixture URLs are pinned to the baseline commit and their SHA-256 values are recorded in [`E2E_TEST_PLAN.md`](E2E_TEST_PLAN.md) and [`verification/manifest.json`](verification/manifest.json).

## Scope and limitations

The MVP handles one sealed tariff revision and one preselected service class. It does not calculate monetary bills, decide legality, resolve customer disputes, manage customer accounts, record a publication chronology or process payments. GenLayer is unnecessary when both documents already expose a signed, machine-readable schema and deterministic comparison is sufficient.

## Repository structure

```text
contracts/utility_tariff_change_notice_covenant.py
tests/test_utility_tariff_change_notice_covenant.py
samples/*.txt
docs/architecture.md
docs/consensus.md
docs/security.md
verification/e2e-matrix.md
verification/manifest.json
E2E_TEST_PLAN.md
requirements.txt
LICENSE
README.md
```
