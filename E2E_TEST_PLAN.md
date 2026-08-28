# Utility Tariff Change Notice Covenant - Studionet E2E Plan

Exact PRE-DEPLOY candidate:

- Contract SHA-256: `637053FF841FA8DC97D43A6F716278CECB43E46F42F6ED7BEFE2A66F45D280D5`
- Tests SHA-256: `ED3A3505515D8BA83BD105AACF1F9BC8215700914394FA023EDB2DDAF4F872F2`
- Network: Studionet, chain ID `61999`

## Scenario matrix

| ID | Transaction path | Expected result and authoritative readback |
|---|---|---|
| E2E-01 | Deploy exact approved source | `FINALIZED`, `SUCCESS`; deployed source hash matches candidate |
| E2E-02 | Owner registers tariff | `read_notice_trace` returns lifecycle `REGISTERED`; required mask `127`; result/evidence `UNRESOLVED`; revision `1` |
| E2E-03 | Non-owner attempts seal | Expected authorization failure; E2E-02 state unchanged |
| E2E-04 | Owner seals tariff | `read_notice_trace` returns lifecycle `TARIFF_SEALED`; sealed identity and required mask unchanged |
| E2E-05 | Permissionless assessment with stable matching tariff/notice | Consensus transaction `FINALIZED`/`SUCCESS`; `TRACEABLE`; masks `127/0/0`; exact day/direction/class/component/evidence readback |
| E2E-06 | Replay exact accepted notice manifest | Idempotent result; revision and state unchanged |
| E2E-07 | Second different notice through initial-assessment API | Expected invalid-transition failure; accepted state unchanged |
| E2E-08 | Non-owner attempts correction | Expected authorization failure; accepted state unchanged |
| E2E-09 | Owner submits stale correction day or reused manifest | Expected deterministic failure; accepted state unchanged |
| E2E-10 | Owner submits newer corrected notice | Consensus transaction `FINALIZED`/`SUCCESS`; `read_notice_trace` returns lifecycle `NOTICE_CORRECTED`; revision increments; history revisions remain readable |
| E2E-11 | Independent record with different service-class notice | Consensus `MISMATCH`; `SERVICE_CLASS` mismatch bit; sealed applicability hash preserved |
| E2E-12 | Independent record with effective-date counterexample | Consensus `MISMATCH`; `EFFECTIVE_DATE` mismatch bit; exact observed date and state readback |
| E2E-13 | Independent record with missing transition condition | Consensus `PARTIAL`; missing-mask bit `64`; complete partition readback |
| E2E-14 | Independent record with unavailable/hash-mismatched evidence | No persistent assessment mutation; record remains sealed; returned evidence failure captured |
| E2E-15 | Invalid registration mask/date/domain/URL representative transactions | Expected deterministic failures; no record created |
| E2E-16 | Read every public view on current and historical revisions | `read_notice_trace`, `read_change_signal`, `read_criterion_masks`, `read_revision` match expected authoritative state |
| E2E-17 | Validator disagreement if reproducibly inducible on Studionet | Transaction reaches protocol `UNDETERMINED` rather than returning contract calldata; authoritative state remains byte-for-byte unchanged. Direct Mode separately proves the validator returns `False`; it cannot simulate network rollback/finality. |

Every successful write must record transaction hash, Explorer URL, `FINALIZED`, `SUCCESS`, validator agreement when consensus runs, and post-transaction readback. Every expected failure must record reproducible transaction/call evidence and before/after state.

## RPC efficiency

- Use one deployment only while the exact source remains unchanged.
- Use separate record IDs on that deployment for traceable, class mismatch, date mismatch, partial and evidence-failure paths.
- Batch deterministic registration/sealing setup before consensus assessments.
- Poll each transaction with bounded intervals until finality; stop polling immediately at terminal state.
- Cache transaction receipts and reuse each authoritative view response across evidence tables instead of repeating identical RPC reads.
- Run deterministic rejection scenarios before expensive consensus scenarios where ordering permits.
- No scenario or required receipt/readback may be omitted to save quota.

## Completion rule

E2E passes only when all applicable rows have actual results and none is `NOT RUN`, `PENDING`, `ASSUMED` or `PARTIAL`; exact deployed revision, finality, execution result, consensus, Explorer evidence and authoritative state must all agree. E2E-17 is marked `NOT APPLICABLE` only if a validator split cannot be induced safely and reproducibly on Studionet; protocol disagreement never counts as a returned `UNRESOLVED` contract result.
