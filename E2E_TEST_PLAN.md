# Utility Tariff Change Notice Covenant - Studio Dev E2E Plan

Exact PRE-DEPLOY candidate: `UTNC-PREDEPLOY-20260923-R5`

- Contract SHA-256: `E5C63156299251C5FE46E554FC2EC94B47951D68C0B100E3D9B0DCA6DA6E897A`
- Tests SHA-256: `9D601821FE70B9E6DA692F95E7A5D6041F3D4800D78FB2B8C2BA940698856721`
- Network: Studio Dev, CLI preset `studio-dev`, GenLayer SDK preset `studioDevnet`, chain ID `61997`, canonical RPC `https://studio-dev.genlayer.com/api`
- Explorer: `https://explorer-studio-dev.genlayer.com/`
- Toolchain compatibility: use matching Studio Dev release-candidate CLI and SDK versions. Before deploy and before the E2E run, record the exact CLI, Testing Suite, SDK and runtime versions plus read-only network/config output. Confirm both explicit presets resolve to the same canonical RPC, chain ID `61997` and Explorer above. Stop before any broadcast if either preset is unavailable, the versions are not a matching supported RC family, or any resolved network value differs; do not relabel `studionet`, use a stable preset, or fall back to a custom endpoint. See [GenLayer Networks](https://docs.genlayer.com/developers/networks) and [Network Configuration](https://docs.genlayer.com/developers/intelligent-contracts/deploying/network-configuration).
- Actor selection: bind one explicit owner actor and one distinct non-owner actor during `studio_tool_readiness`; never use `default`, first-wallet or implicit `from`.
- Clean-run namespace: `UTCNC-20260923-R5`. Before any broadcast, materialize one unique operation ID per write using `utcn-studio-dev-61997-20260923-r5-<scenario>-<write-step>-<actor-alias>-e5c631562992-9d601821fe70`; bind the exact source/test hashes, network, scenario intent, actor address and clean-run ID in the journal. Persist exact IDs before broadcast. Never replace an ID after a hash exists or an outcome is uncertain.

## Executed R5 status

The plan was executed against contract [`0x41CaD4d9BbC0Efe83cf06FE30eBa84b3ceF6Ab42`](https://explorer-studio-dev.genlayer.com/address/0x41CaD4d9BbC0Efe83cf06FE30eBa84b3ceF6Ab42), deployed by explicit owner actor7 in transaction [`0xb0850b4c8f8a90ba8e5fa8151100bc3de34911f1934a4aa98813679505314e40`](https://explorer-studio-dev.genlayer.com/tx/0xb0850b4c8f8a90ba8e5fa8151100bc3de34911f1934a4aa98813679505314e40). E2E-01 through E2E-18 and E2E-20 passed with finalized receipts and authoritative readbacks. E2E-19 is `NOT APPLICABLE`: the documented validator-split preflight returned `sim_call failed (code=-32603): Internal error` before broadcast, and record-019 remained `TARIFF_SEALED`, revision `1`, masks `127/0/0`. See [`verification/e2e-matrix.md`](verification/e2e-matrix.md) for exact transaction hashes, Explorer URLs, semantic payloads and readbacks.

## Immutable public input fixtures

These are synthetic test fixtures, not real utility/tariff evidence and not a claim about any real utility. Each URL is pinned to the exact public baseline commit so the bytes do not drift. Allowlist only `raw.githubusercontent.com` for these records. Hashes are the canonical sample hashes recorded in `verification/manifest.json` at the baseline revision.

Shared registration values: utility `Metro Electric Utility`; jurisdiction `Test Jurisdiction`; tariff revision `Revision 2026-08`; service class `Residential Service`; required mask `127`; permitted domains `["raw.githubusercontent.com"]`.

| Fixture | Exact URL | Expected SHA-256 |
|---|---|---|
| Tariff | `https://raw.githubusercontent.com/pcong5239/utility-tariff-change-notice-covenant/261663a1605af2099e0341fa4a5ebf1b41d90a65/samples/tariff_primary.txt` | `4110eba6ea6ed9c4855483e19e2a4abbe455fc13a47ad1414655310975ac16bd` |
| Primary notice | `https://raw.githubusercontent.com/pcong5239/utility-tariff-change-notice-covenant/261663a1605af2099e0341fa4a5ebf1b41d90a65/samples/notice_primary.txt` | `c17f77d7b7090f225483c09a1b8269e8d0ea091ba7cc48c2a45d0e3d6d65653f` |
| Corrected notice | `https://raw.githubusercontent.com/pcong5239/utility-tariff-change-notice-covenant/261663a1605af2099e0341fa4a5ebf1b41d90a65/samples/notice_corrected.txt` | `927091540e14f1abb26b597ab10e552ad1b25ca9a6e85936aaee54e3259fb5b6` |
| Service-class mismatch | `https://raw.githubusercontent.com/pcong5239/utility-tariff-change-notice-covenant/261663a1605af2099e0341fa4a5ebf1b41d90a65/samples/notice_service_class_mismatch.txt` | `504f18cf804e9431611d1776da9364740342ac11af5a30fb0677d4dbd1882fe2` |
| Effective-date mismatch | `https://raw.githubusercontent.com/pcong5239/utility-tariff-change-notice-covenant/261663a1605af2099e0341fa4a5ebf1b41d90a65/samples/notice_effective_date_mismatch.txt` | `9f2e587e45021674394d3dc7fcdf95df6cfc5fa84817083b9c0deadea00d392a` |
| Partial notice | `https://raw.githubusercontent.com/pcong5239/utility-tariff-change-notice-covenant/261663a1605af2099e0341fa4a5ebf1b41d90a65/samples/notice_partial.txt` | `8720f3f7aa676268d38b6e79cfc44e12392ea2d6b353c8a7949cf652ae224628` |

The common expected assessment fields from the primary fixtures are effective day `20260901`, direction `INCREASE`, service-class hash `4e3f32dc140b547b155e3704633d84a201e0d31449255f6609418186b673ff0a`, component mask `3`, and evidence state `VALID`.

## Scenario matrix

Each pre-state/readback refers to the listed record only. Deterministic failures must have receipt/finality and semantic error reconciled if a transaction is broadcast; before/after public views must be identical. Any client-side schema rejection must be recorded as `NOT BROADCAST` with the exact schema/output and a readback proving no state change. Successful writes require `FINALIZED`, semantic `SUCCESS`, consensus agreement where applicable, and authoritative post-state readback.

| ID | Exact operation input | Expected execution and authoritative readback |
|---|---|---|
| E2E-01 | Deploy exact source above; no constructor arguments. | `FINALIZED`, semantic `SUCCESS`, and deployed byte/source hash exactly matches candidate. |
| E2E-02 | Owner calls `register_tariff("record-001", "Metro Electric Utility", "Test Jurisdiction", "Revision 2026-08", "Residential Service", TARIFF_URL, TARIFF_SHA256, ["raw.githubusercontent.com"], 127)`. | Absent -> `REGISTERED`; required mask `127`, matched/mismatch/missing `0/0/0`, result/evidence `UNRESOLVED`, revision `1`. |
| E2E-03 | Non-owner calls `seal_tariff_revision("record-001")`. | Expected `UNAUTHORIZED`; exact `read_notice_trace` and `read_criterion_masks` unchanged from E2E-02. |
| E2E-04 | Owner calls `seal_tariff_revision("record-001")`. | `REGISTERED` -> `TARIFF_SEALED`; identity, required mask and revision unchanged. |
| E2E-05 | Read-only inspect the exact deployed ABI and confirm the only `assess_customer_notice` arguments are `(record_id, notice_url, notice_sha256)` and the only `reassess_corrected_notice` arguments are `(record_id, prior_revision, notice_url, notice_sha256)`. Compare the legacy calls `assess_customer_notice("record-001", NOTICE_URL, NOTICE_SHA256, 99991231)` and `reassess_corrected_notice("record-001", 1, CORRECTED_NOTICE_URL, CORRECTED_NOTICE_SHA256, 99991231)` against the schema only; do not send or broadcast either. Then the distinct non-owner calls the exact supported shape `assess_customer_notice("record-001", NOTICE_URL, NOTICE_SHA256)` with no date argument. | ABI must contain no caller publication-date argument; both legacy shapes are incompatible and remain `NOT BROADCAST`, with the sealed readback unchanged. Valid call must reach consensus and return `TRACEABLE`, masks `127/0/0`, effective day `20260901`, direction `INCREASE`, service-class hash and component mask as above, evidence `VALID`, revision `1`; current view becomes `NOTICE_ASSESSED`. |
| E2E-06 | Replay the exact E2E-05 call from the same or another caller. | Idempotent accepted tuple; current revision and all stored fields unchanged. |
| E2E-07 | Call `assess_customer_notice("record-001", CORRECTED_NOTICE_URL, CORRECTED_NOTICE_SHA256)` through the initial-assessment API. | Expected `RECORD_ALREADY_ASSESSED`; revision-1 readback unchanged. |
| E2E-08 | Non-owner calls `reassess_corrected_notice("record-001", 1, CORRECTED_NOTICE_URL, CORRECTED_NOTICE_SHA256)`. | Expected `UNAUTHORIZED` before nondeterministic fetch; revision-1 state unchanged. |
| E2E-09 | Owner calls `reassess_corrected_notice("record-001", 2, CORRECTED_NOTICE_URL, CORRECTED_NOTICE_SHA256)` while current revision is `1`. | Expected `STALE_REVISION`; revision-1 state unchanged. |
| E2E-10 | Owner calls `reassess_corrected_notice("record-001", 1, NOTICE_URL, NOTICE_SHA256)` using the exact current manifest first accepted by the non-owner in E2E-05. | Consensus `FINALIZED`/`SUCCESS`; result `TRACEABLE`, same expected assessment fields, lifecycle `NOTICE_CORRECTED`, revision `2`; revisions `1` and `2` remain readable. This proves a permissionless first assessment cannot consume the owner's ability to reassess its current manifest. |
| E2E-11 | Owner calls `reassess_corrected_notice("record-001", 2, CORRECTED_NOTICE_URL, CORRECTED_NOTICE_SHA256)` using a new notice manifest. | Consensus `FINALIZED`/`SUCCESS`; result `TRACEABLE`, same expected assessment fields, lifecycle `NOTICE_CORRECTED`, revision `3`; revisions `1`, `2` and `3` remain readable. No caller-selected publication date is involved in ordering. |
| E2E-12 | Owner calls `reassess_corrected_notice("record-001", 3, NOTICE_URL, NOTICE_SHA256)` to reuse the revision-1 manifest after revisions 2 and 3 have been accepted. | Expected `REUSED_CORRECTION_MANIFEST`; current revision-3 state and all history entries unchanged. |
| E2E-13 | On independent record `record-013`, owner registers/seals with shared tariff values; assess using the service-class-mismatch URL/hash. | Consensus result `MISMATCH`; matched/mismatch/missing `123/4/0`, effective day `20260901`, direction `INCREASE`, component mask `3`, evidence `VALID`; sealed identity remains unchanged. |
| E2E-14 | On independent record `record-014`, owner registers/seals with shared tariff values; assess using the effective-date-mismatch URL/hash. | Consensus result `MISMATCH`; matched/mismatch/missing `119/8/0`, observed day `20261001`, direction `INCREASE`, component mask `3`, evidence `VALID`. |
| E2E-15 | On independent record `record-015`, owner registers/seals with shared tariff values; assess using the partial-notice URL/hash. | Consensus result `PARTIAL`; matched/mismatch/missing `63/0/64`, day `20260901`, direction `INCREASE`, component mask `3`, evidence `VALID`. |
| E2E-16 | On independent record `record-016`, owner registers/seals; assess the primary notice URL using the exact public transaction input that was broadcast: `notice_sha256=0` as an integer/malformed non-string calldata value. The public transaction is [`0xcc63f765...`](https://explorer-studio-dev.genlayer.com/tx/0xcc63f7659eeab81e5fb68c39bbbb24ae32d2325bfd8a2fd617331705ee8eb31f). | Pre-fetch hash validation returns `INVALID_NOTICE_SHA256`; no external fetch or accepted manifest/revision is added; lifecycle remains `TARIFF_SEALED`, revision `1`, and masks remain `127/0/0`. This row does not claim that a 64-character all-zero string was broadcast. |
| E2E-17 | Owner calls `register_tariff("ab", "Metro Electric Utility", "Test Jurisdiction", "Revision 2026-08", "Residential Service", TARIFF_URL, TARIFF_SHA256, ["raw.githubusercontent.com"], 127)`. | Expected `INVALID_RECORD_ID` before storage writes; existing `record-001` readback unchanged. A read using ID `ab` also returns `INVALID_RECORD_ID` because IDs are validated before lookup. |
| E2E-18 | Read `read_notice_trace`, `read_change_signal`, `read_criterion_masks`, `read_revision("record-001", 1)`, `read_revision("record-001", 2)` and `read_revision("record-001", 3)` after E2E-11. | Every current and historical field equals E2E-05/E2E-10/E2E-11 outcomes; no history is overwritten. |
| E2E-19 | Where safely and reproducibly inducible, run a validator split on an independent sealed record with exact payload/source recorded before broadcast. | If induced: protocol `UNDETERMINED`, no contract result returned, authoritative pre/post readbacks identical. Mark `NOT APPLICABLE` only with evidence the Studio Dev path cannot safely and reproducibly induce a split; do not equate `UNRESOLVED` contract output with protocol disagreement. |
| E2E-20 | Owner registers and seals independent record `record-020` with the shared tariff values, then calls `assess_customer_notice("record-020", NOTICE_URL, "g" repeated 64 times)`. | Expected `INVALID_NOTICE_SHA256` before external fetch; finalized semantic error if broadcast; exact sealed-state readback unchanged. |

## Dependency and failure-harvesting map

- E2E-01 is prerequisite for every transaction scenario.
- Core sequence: E2E-02 -> E2E-03/E2E-04 -> E2E-05 -> E2E-06/E2E-07/E2E-08/E2E-09 -> E2E-10 -> E2E-11 -> E2E-12 -> E2E-18. Rows on `record-001` depend on the immediately preceding state named in the matrix; if that record becomes unsafe or differs from expected state, mark only its dependent rows blocked by the exact failure ID.
- E2E-13, E2E-14, E2E-15, E2E-16, E2E-19 and E2E-20 use separate records and are independent of the `record-001` sequence. E2E-17 uses an invalid ID and is independent of all stored records. Continue these safe independent rows while harvesting a core-sequence failure; do not keep executing a row whose own prerequisite failed.

## Run and evidence locks

- For every write, record the exact operation ID, actor alias/address, method and arguments, before-state, expected terminal outcome, transaction hash immediately after broadcast, receipt/finality, semantic execution, consensus result when applicable, after-state and public Explorer URL. Never retry a broadcast or replace an ID while outcome is uncertain.
- Bind E2E-05, E2E-08 through E2E-12 to one explicitly selected owner and a distinct non-owner; no implicit/default actor.
- Each independent counterexample/rejection record uses the shared tariff fixture and its own record ID. Run deterministic invalid-input/authorization/replay paths before expensive consensus paths where prerequisites allow.
- Use bounded polling, cache verified receipt/readback, and stop polling at terminal state. This does not reduce scenario or evidence coverage.
- E2E-19 is the only conditional scenario. All other rows must have actual results; none may remain `NOT RUN`, `PENDING`, `ASSUMED`, `PARTIAL` or `RECONCILIATION_REQUIRED`.
- E2E passes only when exact deployed bytes match the approved contract, all required receipts are `FINALIZED`, semantics and consensus are correct, and authoritative before/after readbacks plus public Explorer evidence match every row.
