# Azure Storage Security Lab — Securing Blob Storage
 
> A hands-on Azure lab demonstrating how to secure Blob Storage — starting
> from an insecure public container, then locking it down properly using
> private access, SAS tokens, and account-level controls.
>

 **Tools used:** Azure Portal · Azure Blob Storage · SAS tokens · storage account security

## What is this project?
 
I built a public blob container that anyone on the internet could read and access without an Azure account. Then, with security in mind, I rebuilt it so the file is private and only accessible with a time-limited token. The lab follows a two-phase story: Phase 1 creates the insecure setup and proves the file is exposed to the public. Phase 2 locks it down step by step and proves,
from a logged-out browser, that an authorization of a token is needed for access of the blob file

## The Architecture
---
```
PHASE 1 — Insecure
 ──────────────────────────────────────────────────────────────

  🌐 Anyone on the internet   ──▶   📦 Public container   ──▶   🐱 File loads, no login


 PHASE 2 — Secured
 ──────────────────────────────────────────────────────────────

  🌐 Public (plain URL)   ──▶   🔒 Private container   ──▶   ❌ Access blocked

  🔑 SAS token holder     ──▶   ⏳ Read-only, expires   ──▶   🐱 File loads

  🛡️ Anonymous Access unselected   ──▶   🚫 No public option   ──▶   Defence in depth
```
---
