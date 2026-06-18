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

## What I Used
 
**Resource Group**
- rg-storage-lab — holds the lab, easy to delete in one go for cleanup

**Storage Account**
- stsecurebloblab — Standard performance, LRS redundancy, West Europe

**Container**
- public-files — a blob container, used to demonstrate public → private

---
## Screenshots

### Phase 1 — Anonymous access enabled on the account
![Phase 1 - Anonymous Access Enabled](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/03%20-%20Phase%201%20-%20Storage%20Account%20Security%20Settings%20(Anonymous%20Access%20Enabled).png?raw=true)

The account-level switch that permits public containers to exist — deliberately enabled in Phase 1 to allow the insecure setup.

### Phase 1 — Creating a public blob container
![Phase 1 - Creating Public Blob Container](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/04%20-%20Phase%201%20-%20Creating%20Public%20Blob%20Container%20(Anonymous%20Read%20Access).png?raw=true)

The container's anonymous access level set to "Blob (anonymous read access)". Azure warns that blobs can be read by anonymous request — flagging the risk as it's created.

### Phase 1 — Proof of exposure
![Phase 1 - Proof of Exposure](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/07%20-%20Phase%201%20-%20Proof%20of%20Exposure%20(File%20Publicly%20Accessible%20in%20Browser).png?raw=true)

The file loaded in a private/incognito browser with no Azure login — proving anyone on the internet with the URL can read it.

### Phase 2 — Container set to Private
![Phase 2 - Container Access Set to Private](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/08%20-%20Phase%202%20-%20Container%20Access%20Set%20to%20Private.png?raw=true)

Changing the container's anonymous access level to "Private (no anonymous access)" to close the public hole.

### Phase 2 — Public access now blocked
![Phase 2 - Public URL Blocked (ResourceNotFound)](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/09%20-%20Phase%202%20-%20Proof%20-%20Public%20URL%20Blocked%20(ResourceNotFound).png?raw=true)

The same URL now returns "ResourceNotFound — The specified resource does not exist." Azure deliberately doesn't reveal the file exists to an unauthorised user.

### Phase 2 — Generating a SAS token
![Phase 2 - Generating SAS Token](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/10%20-%20Phase%202%20-%20Generating%20SAS%20Token%20(Permissions%20and%20Expiry).png?raw=true)

Generating a SAS token with read-only permission, a short expiry, and HTTPS only — least privilege applied to storage access.

### Phase 2 — Controlled access via SAS token
![Phase 2 - File Accessible via SAS Token Only](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/12%20-%20Phase%202%20-%20Proof%20-%20File%20Accessible%20via%20SAS%20Token%20Only.png?raw=true)

The file loads again in incognito — but only with the SAS token appended to the URL. The container stays private; access comes solely from the signed, time-limited token.

### Phase 2 — Public containers blocked at account level
![Phase 2 - Anonymous Access Locked at Account Level](https://github.com/Awezkhan0/Azure-Storage-Security-Lab-Securing-Blob-Storage/blob/main/Screenshots/14%20-%20Phase%202%20-%20Anonymous%20Access%20Locked%20at%20Account%20Level%20(New%20Container).png?raw=true)

After disabling anonymous access at the account level, trying to create a public container is blocked — the access dropdown is locked to Private. Defence in depth: the mistake can't be made.
