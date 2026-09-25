# Security boundary

- Only bounded lowercase IDs, canonical HTTPS URLs, allowlisted domains, SHA-256-style hashes, real calendar dates, closed enums and known bitmasks are accepted.
- Public numeric boundaries reject booleans, strings, floats and out-of-range values instead of coercing them into accepted revisions or criterion masks.
- Sealed utility, jurisdiction, tariff revision, service class and required criteria cannot be changed by assessment or correction.
- Customer, account, meter, address, usage, private-correspondence and amount/rate fields are outside the contract boundary and rejected.
- Source text is untrusted. Contract-authored extraction instructions define the schema; retrieved text cannot add criteria, change enums or override validation.
- Missing or malformed mandatory model fields, including `evidence_state`, fail closed to `UNRESOLVED` and cannot mutate an accepted trace.
- Identity masks are fail-closed and deterministic: the contract derives matched versus mismatch bits from each observed identity hash and the sealed expected hash; model-supplied identity bits cannot override that comparison.
- Authorization and lifecycle checks happen before expensive nondeterministic work.
- Invalid input, source/hash/parsing/VM failure and validator disagreement preserve the prior state.
- Replaying the same accepted assessment is idempotent. Corrections are owner-authorized and revision-bound; the owner may reassess the current manifest, while a manifest accepted only in an older revision is rejected. Callers cannot supply a publication date to consume or lock the correction path.
