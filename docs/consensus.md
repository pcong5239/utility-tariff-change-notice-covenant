# Consensus binding

The validator independently retrieves and checks the hash-bound tariff and notice sources. It exact-compares:

- utility identity and tariff-revision identity;
- matched, mismatch and missing criterion masks;
- effective day;
- charge direction;
- service-class hash;
- billing-component mask; and
- evidence state.

The result label is derived from the partition, not caller input. A mismatch takes precedence over missing criteria; a complete match is `TRACEABLE`; missing criteria without mismatch are `PARTIAL`. Validators also check source meaning, not only JSON shape or the leader's explanation.

If validators cannot agree, GenLayer protocol consensus can leave the transaction undetermined. The contract does not receive a returned tuple from that protocol-level failure, so prior state remains unchanged. Direct Mode tests separately exercise the captured validator's boolean rejection path.
