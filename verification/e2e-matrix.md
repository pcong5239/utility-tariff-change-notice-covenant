# Studionet E2E evidence matrix

Exact deployed revision: `UTCNC-PD-20260825-R3`
Contract: [`0xdcad372A9D2Db1afF6e5fa29A5E3c0EeCAE63814`](https://explorer-studio.genlayer.com/address/0xdcad372A9D2Db1afF6e5fa29A5E3c0EeCAE63814)
Deployer: `0x34b92E6553eaCA11A00A9d86d75d8a7881779D78`
Network: Studionet, chain ID `61999`

All terminal transactions below were checked in Explorer for `FINALIZED`; successful writes also show execution `SUCCESS`. Consensus agreement is recorded where the path reached consensus.

| ID | Evidence | Outcome and authoritative state |
|---|---|---|
| E2E-01 | [deploy tx](https://explorer-studio.genlayer.com/tx/0xb6c3b42f7198bab4617498cb6c2e10c56f5318fd7251b0d4a98e7295991e0638) | `FINALIZED / SUCCESS`; source revision deployed at the contract address. |
| E2E-02 | [register tx](https://explorer-studio.genlayer.com/tx/0x3f371871cd163940b4a374d93ccfb620c33a91c22db7ceecb4e3c9036a13fd23) | `FINALIZED / SUCCESS`; readback `REGISTERED`, required mask `127`, `UNRESOLVED`, revision `1`. |
| E2E-03 | [unauthorized seal tx](https://explorer-studio.genlayer.com/tx/0x7f57c56c3d51272cc690ef135ea2f35f7bbda463b32e54862bcc89b2e3b0f24e) | `FINALIZED`; deterministic `UNAUTHORIZED` rollback; exact before/after readback is recorded below. |
| E2E-04 | [seal tx](https://explorer-studio.genlayer.com/tx/0x997871aea7955265a840842e488c9407d0e6ad1b2423386799de7ffc7c9a500a) | `FINALIZED / SUCCESS / Accepted`; readback lifecycle `TARIFF_SEALED`. |
| E2E-05 | [traceable assessment](https://explorer-studio.genlayer.com/tx/0x907fc1ccfab3bcf90828a9eaf0ec6040051971a7174e8f53de90d420af50e8e2) | `FINALIZED / SUCCESS / Accepted`; `TRACEABLE`, masks `127/0/0`, effective day `20260901`, direction `INCREASE`, component `3`, evidence `VALID`, revision `1`. |
| E2E-06 | [replay assessment](https://explorer-studio.genlayer.com/tx/0xe79cd823f8e13c0cf2853d0a02f0484ba5170f0e36664596eebfc9a3cd07a597) | `FINALIZED / SUCCESS`; idempotent replay; accepted tuple and revision remain unchanged. |
| E2E-07 | [second initial assessment](https://explorer-studio.genlayer.com/tx/0xa9b5556af6fd08e20cf5bf8eb7e6b575e6bf77cc53fdd9f874219ea48ebd7ef6) | `FINALIZED`; deterministic `RECORD_ALREADY_ASSESSED`; exact before/after readback is recorded below. |
| E2E-08 | [unauthorized correction](https://explorer-studio.genlayer.com/tx/0x7cca94fa48c7578cb3dab9db6650df84dd2c487233d129e0b4cd84a6b3a7cab0) | `FINALIZED`; deterministic `UNAUTHORIZED`; exact before/after readback is recorded below. |
| E2E-09 | [reused correction manifest](https://explorer-studio.genlayer.com/tx/0x993d5b8fe2ab6416981d89098d80a25720df96000a46721492213ce82e023f28), [stale revision](https://explorer-studio.genlayer.com/tx/0x0af1199accbd7b0ea78634e8b961d88d1b5f3c31965e1b706dd124562f648e58) | Both `FINALIZED`; deterministic `REUSED_CORRECTION_MANIFEST` and `STALE_REVISION`; exact before/after readback is recorded below. |
| E2E-10 | [corrected notice](https://explorer-studio.genlayer.com/tx/0x1f8690a96e5e11314e7d11c828b253858fda45599553d7da41c570bf185e0def) | `FINALIZED / SUCCESS / Accepted`; `NOTICE_CORRECTED`, `TRACEABLE`, revision `2`; historical revision `1` remains readable. |
| E2E-11 | [service-class mismatch](https://explorer-studio.genlayer.com/tx/0x685002c7f9ffcb3e6781ed7b35b1fbd91846c520ca4f933865d23bfa909ddf65) | `FINALIZED / SUCCESS / Accepted`; `MISMATCH`, matched `123`, mismatch `4`, missing `0`, effective day `20260901`, component `3`, evidence `VALID`; sealed applicability remains intact. |
| E2E-12 | [effective-date mismatch](https://explorer-studio.genlayer.com/tx/0x31f39354366f97641e25a715afd025c9651cd87890ac355c1bf3ebdd94f3a7c1) | `FINALIZED / SUCCESS / Accepted`; `MISMATCH`, matched `119`, mismatch `8`, missing `0`, observed day `20261001`, direction `INCREASE`, component `3`, evidence `VALID`. |
| E2E-13 | [partial assessment](https://explorer-studio.genlayer.com/tx/0x2d41fe4d0d37f0f0bd0e0416606ff0f2558eb5769666963dd4ef3b613a348a72) | `FINALIZED / SUCCESS / Accepted`; `PARTIAL`, matched `63`, mismatch `0`, missing transition bit `64`, effective day `20260901`, direction `INCREASE`, component `3`, evidence `VALID`, revision `1`. |
| E2E-14 | [evidence failure](https://explorer-studio.genlayer.com/tx/0xe064111ca6f100309ed9b9d40089a7e96530f5adbd78af188c27f4980bf4fe8d) | `FINALIZED / SUCCESS / Accepted`; `INSUFFICIENT_PUBLIC_EVIDENCE`; exact before/after readback below proves no persistent assessment mutation and sealed revision `1`. |
| E2E-15 | [invalid registration](https://explorer-studio.genlayer.com/tx/0x3fb8f9b78fb8ed1d3724df7e2693394289e6e6b3ba3f28bd02106d9d0f503cf2) | `FINALIZED`; deterministic `INVALID_RECORD_ID`; no record created. |
| E2E-16 | [contract Explorer](https://explorer-studio.genlayer.com/address/0xdcad372A9D2Db1afF6e5fa29A5E3c0EeCAE63814) | Current and historical `read_notice_trace`, `read_change_signal`, `read_criterion_masks` and `read_revision` matched expected state, including revisions `1` and `2`. |
| E2E-17 | Protocol-disagreement Explorer payload intentionally not linked: independent audit found credential-bearing validator configuration in that payload. Safe public evidence is the [contract Explorer](https://explorer-studio.genlayer.com/address/0xdcad372A9D2Db1afF6e5fa29A5E3c0EeCAE63814). | `FINALIZED`; protocol `UNDETERMINED` after validator disagreement; no contract calldata returned. Exact before/after authoritative readback below proves state unchanged; Direct Mode independently proves validator `False`. |

## Authoritative before/after readback evidence

For each deterministic failure, the following exact public projections were captured immediately before and after the transaction. `M` is `read_criterion_masks(record_id)` and `T` is the selected public tuple `(read_notice_trace[0], read_notice_trace[1], read_notice_trace[2], read_notice_trace[3], read_notice_trace[8], read_notice_trace[9], read_notice_trace[10])`.

| Scenario | Before | After |
|---|---|---|
| E2E-03 unauthorized seal | `M=(127,0,0,0)`; `T=("UNRESOLVED",0,0,0,"UNRESOLVED",1,"REGISTERED")` | identical `M` and `T` |
| E2E-07 duplicate initial assessment | `M=(127,0,0,0)`; `T=("TRACEABLE",127,0,0,"VALID",1,"NOTICE_ASSESSED")` | identical `M` and `T` |
| E2E-08 unauthorized correction | `M=(127,0,0,0)`; `T=("TRACEABLE",127,0,0,"VALID",1,"NOTICE_ASSESSED")` | identical `M` and `T` |
| E2E-09 reused manifest / stale revision | `M=(127,0,0,0)`; `T=("TRACEABLE",127,0,0,"VALID",1,"NOTICE_ASSESSED")` | identical `M` and `T` |
| E2E-14 evidence failure | `M=(127,0,0,0)`; `T=("UNRESOLVED",0,0,0,"UNRESOLVED",1,"TARIFF_SEALED")` | identical `M` and `T` |
| E2E-17 protocol disagreement | `M=(127,0,0,0)`; `T=("UNRESOLVED",0,0,0,"UNRESOLVED",1,"TARIFF_SEALED")` | identical `M` and `T`; no contract calldata |

The `M` tuple is the four-field `(required, matched, mismatch, missing)` view; the first tuple row therefore records the pre-assessment registered state. E2E-17's transaction-level credential-bearing payload is deliberately not published; the contract Explorer and safe state projections remain public.

## Input bindings

The exact public fixtures used for the successful and counterexample paths are preserved in [`samples/`](../samples/): tariff hash `4110eba6ea6ed9c4855483e19e2a4abbe455fc13a47ad1414655310975ac16bd`, primary notice hash `c17f77d7b7090f225483c09a1b8269e8d0ea091ba7cc48c2a45d0e3d6d65653f`, corrected notice hash `927091540e14f1abb26b597ab10e552ad1b25ca9a6e85936aaee54e3259fb5b6`, service-class mismatch hash `504f18cf804e9431611d1776da9364740342ac11af5a30fb0677d4dbd1882fe2`, date mismatch hash `9f2e587e45021674394d3dc7fcdf95df6cfc5fa84817083b9c0deadea00d392a`, and partial notice hash `8720f3f7aa676268d38b6e79cfc44e12392ea2d6b353c8a7949cf652ae224628`.

## RPC efficiency record

One deployment was reused for the matrix. Deterministic setup and rejection paths were run before/alongside consensus paths where state allowed. Transaction polling used bounded intervals with immediate stop at terminal Explorer state. Receipt, consensus, result and readback values were cached and reused in this matrix. Retries occurred only for evidence/terminal-state verification or a nondeterministic consensus outcome; no revision redeploy was performed.
