# Implementation notes

**Approach:** Standard-library Python. Loaders validate and deduplicate each source (first valid record wins; invalid rows never reserve IDs), with total = accepted + invalid + duplicates. The LLM (Gemini/Groq REST) or the offline fixture only supplies `order_id` and `issue`. Python re-validates both against the original message (exactly one distinct `ORD-` + 3-6 digits, model ID must match, unknown issue becomes OTHER). Verdicts and evidence come from three deterministic rules over records sorted by (timestamp, ID).

**Tests:** Smoke output equals `expected.json`; challenge run satisfies count invariants, sorted cases, and evidence only on SIGNAL_FOUND. Unit checks for the 10:10:00 vs 10:10:01 boundary, equal-timestamp shipment, multi-ID complaints, 7-digit IDs, impossible dates.

**Limitations:** Strict exact-match issue labels; no retries or rate-limit backoff; default model names may need updating.

**Risk:** Prompt injection via complaints and support notes. Mitigated by delimiting complaint text as data, never using shipment `details`, and keeping verdicts out of the LLM's hands.

**Live API:** Not tested with a real provider in this environment (no key; network disabled). With no key, every complaint degrades to llm_errors/OTHER per spec. Run locally to produce `result_live.json`.
