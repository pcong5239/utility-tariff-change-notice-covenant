# Utility Tariff Change Notice Covenant

An on-chain, consensus-backed trace for whether a public utility customer notice preserves the consequential fields of a regulator-approved tariff revision.

## Live deployment

- Network: GenLayer Studionet
- Chain ID: `61999`
- Contract: [`0xdcad372A9D2Db1afF6e5fa29A5E3c0EeCAE63814`](https://explorer-studio.genlayer.com/address/0xdcad372A9D2Db1afF6e5fa29A5E3c0EeCAE63814)
- Deployer: `0x34b92E6553eaCA11A00A9d86d75d8a7881779D78`
- Deployment transaction: [`0xb6c3b42f7198bab4617498cb6c2e10c56f5318fd7251b0d4a98e7295991e0638`](https://explorer-studio.genlayer.com/tx/0xb6c3b42f7198bab4617498cb6c2e10c56f5318fd7251b0d4a98e7295991e0638)
- Deployment result: `FINALIZED / SUCCESS`

The live evidence includes a successful `TRACEABLE` consensus assessment ([E2E-05](https://explorer-studio.genlayer.com/tx/0x907fc1ccfab3bcf90828a9eaf0ec6040051971a7174e8f53de90d420af50e8e2)) and a service-class counterexample that returns `MISMATCH` ([E2E-11](https://explorer-studio.genlayer.com/tx/0x685002c7f9ffcb3e6781ed7b35b1fbd91846c520ca4f933865d23bfa909ddf65)). The complete matrix, receipts, consensus outcomes and readbacks are in [`verification/e2e-matrix.md`](verification/e2e-matrix.md).

## Problem and why GenLayer

Tariff sheets and customer notices can use different public wording while referring to the same change. A conventional backend can parse or compare fields, but a single server should not be the sole authority for a consequential public trace. This contract lets validators independently retrieve the hash-bound public sources, extract a fixed set of decision fields and reach consensus before the trace changes.

GenLayer is not needed when both documents already expose a signed, machine-readable schema with exact identifiers and deterministic comparison is sufficient. This contract does not calculate bills, decide legality, resolve customer disputes or process payments.

## How it works

1. The owner registers one tariff revision, its public source identity, allowed domains and required criterion mask.
2. The owner seals the tariff identity; sealed utility, jurisdiction, tariff, service-class and criteria fields are immutable.
3. A permissionless assessment asks the leader and validators to retrieve the tariff and notice sources and produce the same structured decision fields.
4. The validator checks source meaning and exact consequential-field equality independently of the leader's explanation.
5. Only an accepted consensus result mutates the record. The result is derived from the criterion partition: `TRACEABLE`, `MISMATCH`, `PARTIAL`, `INSUFFICIENT_PUBLIC_EVIDENCE` or `UNRESOLVED`.

## State and invariants

Records use one typed `TreeMap[str, TariffNoticeRecord]`. Lifecycle is `REGISTERED -> TARIFF_SEALED -> NOTICE_ASSESSED -> NOTICE_CORRECTED`. Matched, mismatch and missing masks are pairwise disjoint and cover the required mask exactly. Effective date, charge direction, service class, billing component and transition condition are decision fields; explanations are not stored. Replay of the same accepted evidence is idempotent, and corrections require a distinct manifest plus a newer publication day.

## Public API

Writes: `register_tariff`, `seal_tariff_revision`, `assess_customer_notice`, `reassess_corrected_notice`.

Views: `read_notice_trace`, `read_change_signal`, `read_criterion_masks`, `read_revision`.

`read_notice_trace` is the integrator-facing oracle view. It returns the exact masks, observed fields, evidence state, revision and lifecycle needed by notice-review queues, public archives and community-energy tooling.

## Consensus binding and failure behavior

The consensus tuple binds the three criterion masks, effective day, charge direction, service-class hash, component mask, evidence state, utility identity and tariff-revision identity. A generally plausible notice cannot become `TRACEABLE` if it has a different class, date, tariff revision or component. Invalid input and authorization failures revert before nondeterministic work. Source, hash, parsing, VM/LLM or validator disagreement failures preserve the prior record; a protocol-level undetermined transaction does not return contract calldata or mutate state.

## Security and edge cases

URLs are HTTPS-only and domain-allowlisted; hashes, dates, IDs, enum values and bitmasks are bounded. Customer, account, meter, address, usage, private-correspondence and amount/rate fields are rejected. Source text is hostile input: it cannot redefine the prompt schema or criteria. See [`docs/security.md`](docs/security.md) for the boundary and [`docs/consensus.md`](docs/consensus.md) for the exact binding model.

## Tests and reproducibility

```powershell
$env:PYTHONUTF8='1'
$env:GENVM_VERSION='v0.2.16'
python -m pip install -r requirements.txt
python -m pytest tests/test_utility_tariff_change_notice_covenant.py -q
genvm-lint check contracts/utility_tariff_change_notice_covenant.py
python -m compileall -q contracts tests
```

The authoritative local Direct Mode run used WSL2/uv with Python 3.12 and the exact pins in [`requirements.txt`](requirements.txt): `105 passed`, compileall passed, and `genvm-lint` passed 3 checks with semantic validation. Reproduce it from WSL2 with:

```bash
uv run --with-requirements requirements.txt python -m pytest tests/test_utility_tariff_change_notice_covenant.py -q
uv run --with-requirements requirements.txt python -m compileall -q contracts tests
GENVM_VERSION=v0.2.16 uv run --with-requirements requirements.txt genvm-lint check contracts/utility_tariff_change_notice_covenant.py
```

Native Windows `genlayer-test==0.29.2` Direct Mode currently fails before contract import while its loader unlinks an open stdin temporary file (`PermissionError: [WinError 32]`) under both CPython 3.12 and 3.13. This is a tooling/OS limitation, not a passing Windows test result; use WSL2 for the reproducible Direct Mode run. The exact deployed source, test and E2E-plan hashes are recorded in [`verification/manifest.json`](verification/manifest.json). [`E2E_TEST_PLAN.md`](E2E_TEST_PLAN.md) records the complete 17-scenario plan.

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
.gitignore
README.md
```

## Consensus engineering lessons

- Bind every consequential field, not only the coarse result label.
- Derive status from a complete, disjoint criterion partition.
- Treat public evidence and model output as hostile input.
- Keep storage mutation after consensus acceptance and preserve state on failures.
- `FINALIZED` is necessary but must be paired with execution `SUCCESS` and authoritative readback.

## Reusable integrations and limitations

Integrators can use `read_notice_trace` to flag a notice for manual review, feed a public tariff archive, or route a community-energy compliance queue. The MVP handles one sealed tariff revision and one preselected service class; it does not compute monetary rates, interpret law, manage customer accounts or crawl sources.
