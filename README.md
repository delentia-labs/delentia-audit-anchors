# Delentia audit anchors

This repository is a **witness**. A Delentia runtime that keeps a hash-chained, signed audit log publishes the *head* of that chain here from time to time.
Each line of `anchors/<key_id>.jsonl` is one signed head: the key id, how many audit rows existed, the hash of the newest row, the time it was signed, and an Ed25519 signature.

**What is here:** signed chain heads and counts. **What is not here:** any content of the log, any goal, any answer, any personal data, any key.

## Why it exists
A signed hash chain proves a log was not edited *unless* someone holds the signing key and rewrites the whole log. A copy of the head kept somewhere the host cannot rewrite closes that gap:
if the log is later rewritten, row N of the new log will not hash to the head recorded here for N, and a shorter log cannot extend the record.
The branch is protected against force-push and deletion, so the history of this file is itself part of the evidence.

## How to check a log against it
`delentia audit-chain check-witnesses` (or `scripts/verify_audit_bundle.py` in the Delentia-OS repository, which needs only Python and `cryptography`). A rewritten row is reported as `row N hashes to ..., anchored ... (rewritten)`.

## What this does and does not prove
It makes tampering by someone who controls the runtime's machine **detectable** (tamper-evident). It does not make the log tamper-proof, it does not prove the entries are true, and it only covers rows up to the newest anchor.
