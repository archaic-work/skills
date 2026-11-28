# HTTP request diagnostics: flight recorder

## Diagnostic objective

Use a request-scoped flight recorder as the default logging architecture for Archaic Go HTTP servers. Instrument for the investigation of a failed request, not for the volume of ordinary log output. Aim to explain the failure, its preceding decisions and the outcome of surrounding operations from a single correlated report, without reproducing the request or changing the log level.

Use `log/slog` and the standard library. Prefer a small application-local implementation; extract a reusable library only after multiple applications demonstrate a need. Do not impose request buffering on CLI tools or background-only processes. Log startup, shutdown and background operations on their own lifecycle paths. Keep mandatory audit/security records separate from disposable request diagnostics.

## Capture and emission policy

- Capture structured events at DEBUG, INFO, WARN and ERROR from request entry through response completion. Discard successful request recordings; replay failed requests once at the outer request boundary in chronological order, with original timestamps, levels, attributes and correlation. Do not use the sink's normal level threshold to filter captured or replayed events.
- Use levels semantically: DEBUG for technical steps and observations; INFO for significant state changes or completed operations; WARN for unexpected but recoverable conditions; ERROR for failed operations. Decide retention from explicit failure state, not the level threshold or HTTP status alone.
- Mark failures explicitly, including application and dependency errors even when handled, 4xx/5xx responses, panics, cancellation, deadline expiry, incomplete streams, and response-write or flush failures. Distinguish an expected business outcome from an actual failed dependency; do not silently discard a handled failure because the final response is successful.
- Finalize after response handling and panic recovery, including headers already sent and streaming responses. Preserve the server's panic policy. Keep one final summary with the overall outcome, failure reasons, elapsed time, HTTP status, response bytes, and dropped/aggregated-record counts where applicable.
- Generate or validate an opaque request correlation ID at the trust boundary, expose it in the response where appropriate, and associate the build/revision with the recording. Keep correlation throughout application and dependency work; propagate an appropriate ID to cooperating dependencies where supported and safe.

## Instrument the whole request

Treat diagnostic coverage as part of implementation, not as optional instrumentation added after a failure. Record successful steps as deliberately as failed ones. For each relevant operation, capture its start or decision, safe input metadata and effective configuration, then its result, duration and safe output metadata. Record enough detail to distinguish which operation was attempted, what influenced its behavior, where it stopped and what happened afterward. Use monotonic elapsed time for durations and consistent units in attribute names.

Adapt the following coverage to actual application behavior; do not invent operations just to emit logs:

| Area | Diagnostic evidence |
| --- | --- |
| Entry and routing | Request ID, build, method, static route pattern, known request size |
| Authentication and authorization | Session/credential validation outcome, authorization decision and rejection reason, including successful checks |
| Parsing and validation | Safe input sizes/counts, selected options, validation decisions and limits |
| Admission and planning | Resource availability, selected execution path, effective non-sensitive configuration and budgets, reasons for fallback or rejection |
| Persistence | Operation, record count or size, transaction/commit outcome, elapsed time, classified failure |
| Dependency calls | Logical dependency and operation, safe request metadata, connection/response timing, HTTP or protocol status, attempts and fallback outcomes |
| Long-running or streamed work | Start, time to first useful output, phase transitions, periodic aggregate progress, final counters, protocol completion/termination reason |
| Application result | State transition, complete/partial/failed/cancelled outcome, persistence or cleanup result |
| Response completion | Actual status, bytes written, stream completion, cancellation/write failures and total elapsed time |

Use fixed, searchable event names and structured attributes, not formatted prose containing values. Make branches, fallback decisions, retries and cleanup observable. Avoid logging every function entry/exit when it adds no diagnostic value. Keep unsuccessful optional operations visible even if a fallback succeeds.

For streaming protocols, distinguish transport success from protocol and application completion. Record the negotiated media/protocol type in safe normalized form, time to first event and first useful output as distinct milestones when relevant, event and byte counts, available budget/usage counters, terminal protocol markers and classified termination reasons. Distinguish an upstream limit, upstream error, malformed frame, missing terminal marker, connection failure, cancellation, timeout, local limit and downstream write failure when applicable. Do not claim a token count or termination marker that was not observed.

## Meaningful errors

- Classify errors at the layer that knows their semantics. Use sentinel/typed errors and wrapping with `%w`, preserving `errors.Is`/`errors.As` and underlying cancellation, timeout or transport information. Do not parse arbitrary error strings for classification.
- At the handling boundary, record a stable operation, phase when useful, error category and specific reason. Add safe protocol/status codes, relevant counts/limits, elapsed time and retry disposition when known. A Go type such as `*errors.errorString`, a generic `internal` category or a generic failure message is not sufficient when the code knows a more specific cause.
- Preserve the specific distinction between locally generated errors and errors from external dependencies. Allow known static internal error descriptions or codes; classify external values against an explicit safe vocabulary and use `unknown` for unrecognized values. Never log an untrusted `Error()` string, arbitrary protocol error body or arbitrary upstream reason verbatim.
- Log a failed operation once at its handling boundary; use diagnostic events at lower layers to explain the preceding work and carry structured cause information upward. Preserve subsequent outcomes such as partial-result persistence or cleanup. Do not count the same propagated error as multiple independent failures.

Example (illustrative, not a prescribed schema):

```text
DEBUG request.routed route="POST /api/jobs/{id}/run"
DEBUG auth.completed authenticated=true authorized=true
DEBUG input.validated input_bytes=284
DEBUG dependency.started operation=generate output_limit=2048 timeout_ms=30000
DEBUG dependency.connected status_code=200 content_type=text/event-stream elapsed_ms=12
DEBUG stream.first_output elapsed_ms=320
DEBUG stream.terminated finish_reason=length events=340 output_bytes=6184 terminal_marker=false
ERROR operation.failed operation=generate phase=consume_stream error_kind=upstream_limit error_reason=output_limit_reached duration_ms=842
INFO storage.completed operation=save_result result=partial duration_ms=5
ERROR request.failed status_code=200 outcome=partial duration_ms=865 dropped_records=0
```

Attach request correlation and timestamps to every event. The report must distinguish an observed fact from an inferred cause; do not manufacture precise reasons when only a generic failure is known.

## Detail without unbounded cost

A flight recorder avoids normal log I/O and retention, not the CPU and memory cost of creating records. Capture all diagnostically useful decisions and milestones; aggregate high-frequency activity (tokens, chunks, individual reads, repetitive retries) into counters, timings and bounded progress snapshots. Emit phase changes promptly. Use a time- or count-based sampling policy for repetitive progress; always capture the final totals and outcome, including on failure. Avoid expensive serialization, payload copying or extra network requests solely for logging. Measure normal and failure-path overhead under representative concurrency and long-lived requests.

- Bound retention per request by record count and/or bytes, including attribute values, and account for concurrent requests in the application's resource budget. Choose limits based on representative measurements, not a universal constant.
- Preserve both the initial context and the events immediately preceding failure: retain a bounded prefix and a rolling tail (or an equivalent retention policy). Do not use a drop-new-only buffer that loses the events most relevant to a late failure.
- Protect significant failure evidence and the final summary against ordinary progress eviction. Bound this protection as well: coalesce or count repeated failures, retain representative early and recent distinct failure information, and report any loss. Do not promise unlimited retention or allow unlimited error records.
- Replay retained events in their original capture order, without duplicates; make gaps and truncation explicit. Report overwritten/dropped records separately from deliberate aggregation where meaningful. Release retained data after finalization.

## Data protection

Redact and minimize before buffering, not only before replay. Treat temporary memory as a sensitive destination too. Use explicit allowlists of diagnostic event names, attribute keys, value types, safe enum values and bounded lengths/sizes; an allowed key or a truncated string alone does not make an arbitrary value safe. Resolve `slog.LogValuer` values safely before capture. Review nested groups and bound their depth/size when supported.

Prefer static operational names, normalized protocol codes, counts, durations, flags and safe configured values. Exclude credentials, cookies, authentication headers, tokens used as credentials, personal identifiers, raw URLs/query strings, payloads, user content, private intermediate content, untrusted error messages and stack data that may expose secrets. Never include a raw configuration dump. Use a logical dependency name and approved configuration fields rather than its raw URL or full request. Use the same safeguards for failure summaries, late events and ordinary logging paths. Restrict access to emitted reports and apply appropriate operational retention.

## Recorder lifecycle and correctness

- Put middleware at a boundary that observes routing, recovery, cancellation and response completion. Observe errors from `Write` and flush/stream operations even after headers are sent; preserve required `http.ResponseWriter` behavior and response-controller compatibility.
- Enable DEBUG capture in the request-scoped `slog.Handler.Enabled`. Replay through a sink path that preserves the captured levels regardless of normal sink filtering. Preserve `WithAttrs` and `WithGroup` semantics and clone retained records; snapshot mutable values safely.
- Isolate and synchronize concurrent recordings. Propagate context to request-owned work, join it before finalization, and define ownership when work outlives a request. Do not let late goroutines race finalization or silently reopen the buffer. Handle lifecycle violations safely and with correlation.
- Keep failure marking and finalization idempotent. Discard recordings on success and flush on failure exactly once. Preserve original record ordering and bounded memory even with concurrent writers and repeated errors.

## Verification and compliance review

Treat diagnostic completeness as a testable quality requirement, not merely the existence of a recorder. For new or modified HTTP behavior, review the entire relevant request lifecycle and exercise success and failure fixtures. Check that the emitted report explains the cause and important surrounding successful operations, not just that a line at ERROR exists.

- Verify silent successes and exactly-once failure replay through DEBUG, including original ordering, levels, correlation, and handling of grouped attributes. Cover handled dependency errors and failures after a successful HTTP status.
- Exercise distinct failure modes, protocol truncation/disconnection, timeouts, cancellation, response-write/flush errors, panics, persistence and cleanup failures where applicable. Assert specific reason codes, preceding diagnostic milestones, useful counters/limits and the final application outcome. Use real HTTP handlers and controlled dependency fixtures; avoid brittle full-log snapshots.
- Verify overflow retains initial context, recent events and bounded failure evidence; verify loss accounting, chronological replay, bounded high-frequency progress and final counters. Test redaction before capture, unknown/malicious values, nested attributes, concurrent isolation, late work and finalization. Run race detection for concurrent recorder changes.
- Measure or explicitly account for overhead and resource bounds under representative request lengths and concurrency. Report checks not run.

When asked for a compliance review, inspect actual source, configuration and tests, not only documentation. Trace representative routes and dependency/streaming paths end to end; compare their diagnostic coverage against the table and requirements above. Report each material gap with file/symbol evidence, operational consequence, and a minimal proposed remediation plus acceptance test. Distinguish verified conformance, nonconformance, justified non-applicability and unverified behavior. Prioritize missing causal evidence and unsafe/unbounded recording before cosmetic log changes. A review alone does not authorize implementation; propose a concrete, ordered implementation plan unless changes are requested. Do not treat passing existing tests or the mere presence of flight-recorder middleware as proof of compliance.
