# Pre-Authentication XSS Leading to OS Command Execution in an Electron Client

**Status:** Confirmed — reproduced in a real desktop Electron client  
**Severity:** Critical  
**CVSS v3.1:** 9.6 (indicative numerical assessment)  
**Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:H`  
**Weaknesses:** CWE-79 (Cross-Site Scripting), CWE-94 (Code Injection)

## Vulnerability

A staff-facing login page reflects the user-controlled `u` redirect parameter into an HTML input attribute without context-appropriate escaping. An attacker can terminate the attribute and inject executable HTML, resulting in **pre-authentication reflected XSS**.

The same login page can be rendered within a desktop application built on Electron. Testing in the **real Electron client**, not merely a simulated renderer, confirmed that injected page JavaScript could access Node.js `require`, load `child_process`, and execute a local operating-system command under the account running the desktop client.

This forms a confirmed chain from a remotely supplied login URL to **operating-system command execution on a user's workstation** when that URL is opened in the affected Electron client.

## Technical Details

**Affected login route:**

```http
GET /cashbox.php/vtLogin/login?u=<URL_ENCODED_PAYLOAD> HTTP/1.1
```

The underlying output-encoding flaw allows an attacker-supplied value to break out of a hidden input's `value` attribute and create an executable HTML/SVG element.

Once this JavaScript runs inside the affected Electron renderer, exposed Node.js APIs allow access to the operating system:

```javascript
const output = require('child_process')
  .execSync('whoami')
  .toString()
  .trim();
```

The vulnerability does not require authentication to construct the malicious login URL. Execution in the desktop client requires the affected page to be opened or rendered there.

## Confirmed Evidence

The real-client validation established the following results:

```text
JavaScript execution from injected login parameter: confirmed
Electron renderer context:                    confirmed
Node.js require:                             available
child_process:                               available
Command executed:                            whoami
Operating-system command execution:          confirmed
Command output:                              current Windows user
```

The benign command returned the Windows account running the client. This demonstrates OS-level execution rather than browser-only XSS. No reverse shell, persistence mechanism, credential extraction, or destructive operation was needed for the proof.

## Impact

Successful exploitation allows an attacker to execute operating-system commands with the privileges of the user running the vulnerable Electron application. Depending on those privileges, follow-on actions may affect files, credentials, and other resources accessible to that account. The proof confirmed command execution; it did not attempt those additional actions.

## Root Cause and Remediation

The exploit chain combines **unescaped attacker-controlled HTML attribute content** with **Node.js APIs exposed to web content inside Electron**.

- Encode `u` for its HTML attribute context and allowlist redirect destinations.
- Disable Node.js integration in renderers that load remote or untrusted pages.
- Enable Electron context isolation and renderer sandboxing.
- Do not expose `require`, `process`, `child_process`, or general-purpose IPC to page JavaScript; restrict preload interfaces to explicitly needed actions.
- Introduce regression tests for both the login page's reflected XSS and the Electron renderer's privilege boundary.
