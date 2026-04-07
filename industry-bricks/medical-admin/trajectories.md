# Logic-Trace Trajectories — Medical Prior Authorization

> Execution index for the medical-admin industry brick.
> Reference this file before modifying `server.py`.

---

## [Flow] Prior Authorization Request

1. Doctor Agent: `POST /prior-auth` [body: icd_10_code, requested_service, clinical_note]
2. `logic_trace_scan(clinical_note)` — scans for red flag keywords
3. Policy Check: `"MRI" in requested_service AND (pt_completed OR flags)`
4. Decision: `APPROVED | PENDING_URGENT_REVIEW | DENIED`
5. Truth-Trace: `SHA-256(icd_10_code + requested_service + status + timestamp)`
6. Response: JSON [status, metadata, provenance hash]

## [Side-Effects]
- Integrity hash generated and attached to every response
- Timestamp recorded at decision point

## [Known Error Surfaces]
- `DENIED`: No PT completion and no red flags detected
- `PENDING_URGENT_REVIEW`: Red flags present but MRI not requested
- `500`: FastMCP server not running or import error

## [Known Limitations]
- Red flag detection is keyword-based, not semantic
- Payer policy logic is hardcoded, not pulled from live API
- No patient_id handling in current implementation

## [Stack References]
- Schema: [prior-auth-schema.md](prior-auth-schema.md)
- Verification: [Truth-Trace Protocol](https://github.com/uameer/Truth-Trace-Protocol)
