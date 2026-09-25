# Architecture

`UtilityTariffChangeNoticeCovenant` is a standalone GenLayer Intelligent Contract with a typed current/history record map and a per-record accepted-manifest index:

`TreeMap[str, TariffNoticeRecord]`

`TreeMap[str, bool]` (`record_id:manifest_hash`)

The owner registers and seals a tariff identity. A permissionless assessment then performs the nondeterministic retrieval and extraction work inside the Equivalence Principle boundary. The leader and validators return a bounded tuple of decision fields. Storage and the accepted-manifest index are updated only after consensus acceptance. Corrections preserve the sealed tariff identity and require the owner's authorization and current revision. The owner may reassess the current accepted manifest or use a manifest never accepted for that record; reuse of an older historical manifest is rejected. Each accepted correction appends a notice revision. No caller-supplied publication date controls correction order.

The contract deliberately has no frontend, billing engine, amount calculation, customer database, payment flow, crawler or legal determination. The only reusable integration surface is the public read API, especially `read_notice_trace`.
