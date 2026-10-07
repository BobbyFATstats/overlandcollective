# Bird Dog Guide form → Orbit (2026-10-07)

The Bird Dog Guide page embedded a GoHighLevel form posting to a cancelled GHL account, so sign-ups were lost.
Replaced with a native form (First Name + Email required, Phone optional, unticked opt-in with the old GHL
wording, honeypot) that POSTs JSON to Orbit `https://orbit.overlandparkscollective.com/api/resources/request`
(Orbit PR #132, ADR 0045). Orbit stores the contact (tags, consent, activity) and emails the guide from
info@overlandparkscollective.com.

- Errors map per field; 429 / timeout (15 s, AbortController for old Safari) / network → friendly form-level message.
- Success fires GA4 `generate_lead` with form `bird_dog_guide`.
- Verified end-to-end against prod before merge (local Orbit + local site); shipped only after the endpoint was live.
