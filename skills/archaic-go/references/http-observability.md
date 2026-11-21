# HTTP request flight recorder

Use this pattern for Archaic Go HTTP server applications when request-scoped diagnostics must be retained on failure without logging every successful request. Keep startup/background events on ordinary logging paths, not in request-local buffers.

- For HTTP server applications, use the flight recorder log pattern: collect structured events through debug level in a buffer owned by each request, starting at request entry and continuing through response completion. Discard the buffer on success; emit it only when something goes wrong during that lifecycle.
- Record failure explicitly, including application or dependency errors even when handled, error responses (4xx/5xx), panics, cancellation or deadline expiry, and response-write failures. Do not infer success solely from the HTTP status; a response may fail after headers have been sent.
- Finalize at the outer request boundary, after response handling and panic recovery. Flush failed requests once, in recorded order, with original levels, timestamps, attributes, request correlation, and the failure reason. Preserve the server's panic policy.
- Enable debug capture in the recorder's `slog.Handler.Enabled`; do not let the output handler's normal level threshold discard buffered debug records during replay. Preserve `WithAttrs` and `WithGroup` semantics and clone records before retaining them.
- Bound each request's buffer by records or bytes, report truncation when flushing, and synchronize access from concurrent request work. Keep buffers isolated and define ownership through completion so late goroutines cannot race finalization or silently lose failure events.
- Redact sensitive data before buffering and release retained data on completion. Keep startup and background logging outside the request recorder.
- Test that successful requests emit nothing, every failure path flushes diagnostics through debug exactly once, and concurrent requests remain isolated. Include buffer overflow, panic recovery, cancellation, and response-write failures.

## Adoption

- Place middleware and the finalization boundary where they can observe response-write errors, recovery, and cancellations; do not assume status code alone proves success.
- Start with a bounded, straightforward implementation. Introduce a reusable library only if multiple applications demonstrate a need.
- Verify the behavior with real handlers and concurrent requests; keep sensitive payloads out of buffered log attributes.
