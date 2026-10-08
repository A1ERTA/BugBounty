# ZIP Extraction Symlink Traversal Leading to Server-Side Code Execution

**CVSS v3.1:** 10.0 (Critical) — *indicative score*

## Vulnerability

A ZIP-based plugin importer failed to restrict extraction paths and symbolic links. When an archive included a symlink followed by another entry using the same path, the importer could write through the link and overwrite a JavaScript plugin outside the extraction directory.

## Evidence

1. A controlled lab application accepted an archive encoded in `zipbase64` through its plugin-import workflow.
2. The archive included a symbolic link from `../redis.js` to `../../plugins/redis.js`, followed by a file entry at the same archive path.
3. The importer combined the extraction directory and archive entry name with `path.join(dest, entry.fileName)` but did not enforce a canonical containment check.
4. When the modified plugin was loaded through `pluginToRun`, the server executed the injected JavaScript. Reading a local test file was confirmed.

## Impact

The archive import permitted writes outside the intended directory and execution of attacker-supplied Node.js code under the application's process privileges. The demonstration was performed in a controlled training environment.
