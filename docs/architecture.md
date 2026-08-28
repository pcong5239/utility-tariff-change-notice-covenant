# Architecture

`UtilityTariffChangeNoticeCovenant` is a standalone GenLayer Intelligent Contract with one persistent typed map:

`TreeMap[str, TariffNoticeRecord]`

The owner registers and seals a tariff identity. A permissionless assessment then performs the nondeterministic retrieval and extraction work inside the Equivalence Principle boundary. The leader and validators return a bounded tuple of decision fields. Storage is updated only after consensus acceptance. Corrections preserve the sealed tariff identity and append a new notice revision.

The contract deliberately has no frontend, billing engine, amount calculation, customer database, payment flow, crawler or legal determination. The only reusable integration surface is the public read API, especially `read_notice_trace`.
