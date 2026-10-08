# ERB Server-Side Template Injection Through an Email Parameter

**CVSS v3.1:** 9.8 (Critical) — *indicative score*

## Vulnerability

An invitation generator inserted an untrusted `email` query parameter into **ERB template source** before compiling the template. ERB directives in the supplied text therefore executed server-side rather than being treated as an email address string.

## Evidence

1. The controlled training application accepted an email address containing a quoted display name with embedded ERB syntax.
2. URL encoding and valid email syntax allowed the expression to pass through the email parser and reach the template compilation stage.
3. A benign file-read demonstration used `<%= File.read('/tmp/app/flag.txt') %>` and displayed the local file content in the generated email-recipient field.

## Impact

Untrusted input reached a fully executable Ruby template context. Arbitrary server-side file read was demonstrated. Additional Ruby method calls, potentially including OS-command execution, were plausible but were not separately verified.
