# Security boundary

- Only bounded lowercase IDs, canonical HTTPS URLs, allowlisted domains, SHA-256-style hashes, real calendar dates, closed enums and known bitmasks are accepted.
- Sealed utility, jurisdiction, tariff revision, service class and required criteria cannot be changed by assessment or correction.
- Customer, account, meter, address, usage, private-correspondence and amount/rate fields are outside the contract boundary and rejected.
- Source text is untrusted. Contract-authored extraction instructions define the schema; retrieved text cannot add criteria, change enums or override validation.
- Authorization and lifecycle checks happen before expensive nondeterministic work.
- Invalid input, source/hash/parsing/VM failure and validator disagreement preserve the prior state.
- Accepted evidence is replay-idempotent. A correction requires a new manifest and a newer publication day.
