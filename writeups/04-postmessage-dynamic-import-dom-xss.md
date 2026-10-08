# DOM XSS Through postMessage and Unrestricted Dynamic import()

**CVSS v3.1:** 6.1 (Medium) — *indicative score*

## Vulnerability

An embeddable page allowed its parent origin to be supplied in a query parameter. Its `page.resolve` message handler then passed the untrusted `resolvePageScriptUrl` field directly to JavaScript's dynamic `import()`. The handler did not restrict URL schemes or allowed module locations.

## Evidence

The vulnerable pattern was equivalent to:

```javascript
const origin = decodeURIComponent(params.get('origin') || '');
transport.addPeer({ id: '1', source: window.parent, origin });
handler.addHandler('page.resolve', async request => {
  const module = (await import(request.resolvePageScriptUrl)).default;
  return { pageName: module(request.slotType, request.context, request.extSettings) };
});
```

1. A controlled external page embedded `/iframe/pageResolver?origin={attackerOrigin}`.
2. It sent `page.resolve` with a `data:text/javascript` module URL.
3. The module returned `location.origin` through the RPC response, confirming code execution in the iframe application's origin.
4. A follow-up module requested `/api/v1/config/initial` from that origin and returned the response through the parent messaging channel; the observed response included a WebSocket token.

## Impact

Arbitrary JavaScript executed in the iframe's origin, with access to same-origin API responses and an existing messaging path back to the initiating page. Access to unrelated origins or an authenticated primary-domain account was not independently verified.
