# CDP Capture

The agent controls Chrome via raw WebSocket using the CDP protocol. Chrome runs visibly (not
headless) to avoid bot detection and rendering issues. The agent drives the browser autonomously.
The user only intervenes when the agent is genuinely stuck -- a CAPTCHA, a login wall with no
provided credentials, or similar.

## Output format

Intercepted requests are written to `artifacts/{timestamp}/requests.ndjson` -- one JSON object per
line. The file is append-safe; stopping and restarting does not lose prior captures.

Each entry should contain enough to fully reproduce the request: method, URL, headers, post body,
and the corresponding response status, headers, and mime type.
