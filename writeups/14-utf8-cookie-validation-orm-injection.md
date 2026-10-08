# UTF-8 Byte-Length Mismatch Enables Session and ORM Query Injection

**CVSS v3.1:** 6.5 (Medium) — *indicative score*

## Vulnerability

A JavaScript session-cookie validator mixed the string's character length (`session.length`) with the raw UTF-8 bytes from `Buffer.from(session, 'utf8')`. Multibyte characters caused the validation loop to inspect an incomplete byte range. The unvalidated remainder was then interpolated into JSON used as a Sequelize query filter.

## Evidence

1. A crafted session cookie began with repeated multibyte characters (for example `é`) and ended with JSON-altering content.
2. The byte-length mismatch let the suffix survive the input filter.
3. The parsed cookie contained an attacker-controlled `session.id=1` rather than the intended criteria.
4. A subsequent `Users.findOne({ where: cookie.session })` operation returned a different user's record in the training environment.

## Impact

An attacker could alter the effective ORM query by manipulating a session cookie, bypassing expected record selection and reading data belonging to another user.
