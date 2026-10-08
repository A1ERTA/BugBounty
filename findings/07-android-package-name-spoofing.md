# Android Package-Name Spoofing Exposes a Private Media Library

**CVSS v3.1:** 4.7 (Medium) — *indicative score*

## Vulnerability

An exported Android `MediaBrowserService` relied on the caller's package name as an allowlist check. A third-party APK could declare the name of a trusted test application, making a plain package-name comparison insufficient to authenticate the caller.

## Evidence

1. A test APK was built with the allowed package name `androidx.media3.testapp.controller`. No privileged Android permissions were necessary.
2. After installation on a device with an authenticated media application, the APK connected to the exported `AudioPlayerService`.
3. The service returned personal-library categories such as saved books, downloads, and custom lists, including book titles, authors, internal IDs, and cover URLs.
4. An otherwise equivalent APK with a different package name could not connect.

## Impact

A third-party app installed on the same device could retrieve personal media-library metadata without credentials for the target application. Exploitation required installing an APK on the affected device; remote access without installation was not demonstrated.
