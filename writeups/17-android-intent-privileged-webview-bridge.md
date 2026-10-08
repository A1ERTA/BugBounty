# Android Intent Injection Grants Access to a Privileged WebView Bridge

**Vulnerability:** Improper validation of externally supplied Android Intent data / unsafe WebView JavaScript bridge exposure  
**CVSS v3.1:** 4.4 — Medium (indicative)  
**Vector:** `CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:N/I:L/A:L`

## Vulnerability

An exported Android `MainActivity` accepts untrusted notification extras from other applications on the device. The `notification_web_url` value is treated as navigation input without restricting it to trusted HTTPS origins. Consequently, another installed application can cause the target app to load a `data:text/html` document inside a WebView with JavaScript enabled and an exposed native interface, `Android.receiveMessage`.

This crosses an Android app-to-app trust boundary: attacker-controlled JavaScript is executed in the target application's WebView and can dispatch messages to its native event handlers. The attacker does not need root access, accessibility privileges, access to the target app's files, or the target user's credentials; they do need an application capable of sending the Intent on the same device.

## Technical evidence

1. The launcher activity was confirmed to be exported and to forward notification extras to its navigation logic.
2. Supplying a `data:text/html` URL as `notification_web_url` loaded attacker-controlled JavaScript in a secondary WebView.
3. The document exposed `Android.receiveMessage` as a callable function.
4. The injected JavaScript submitted a `RefreshSession` event:

```javascript
Android.receiveMessage('{"event":"RefreshSession"}');
```

5. The native callback invoked `refreshAuthenticationAndReload()`, which entered the authentication/cookie refresh path and reloaded the WebView. After the reload, the injected document reported:

```text
reloaded after RefreshSession; bridge=function
```

6. A separate `OpenBrowser` test reached the native browser-opening handler. An `OpenDocuments` event did not perform navigation in the tested secondary WebView because its callback was unavailable.

**Entry point:** exported Android activity → untrusted `notification_web_url` → WebFlow/WebView → JavaScript bridge → native event dispatcher.

## Confirmed impact

- Execution of attacker-supplied JavaScript in a privileged application WebView.
- Invoking the native `RefreshSession` handler and triggering an authentication refresh/reload.
- Invoking the native `OpenBrowser` handler.

**Limitations:** No credential extraction, document access, account takeover, financial action, or other sensitive bridge operation was demonstrated in this test. A separate write-up addresses the independently verified session-token exposure reachable through the same underlying bridge design.

## Root cause

Externally supplied IPC/navigation data is trusted without strict scheme/host validation, while the resulting WebView is given access to native functionality intended for trusted application content. A secure design should separate untrusted navigation from privileged WebViews, validate the source of notification Intents, and authorize sensitive bridge actions based on a trusted content origin.
