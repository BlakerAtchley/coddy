# Cybersecurity: Investigating a Breach

**Coddy Cybersecurity journey, Section 2** · 4 chapters · 20 scenario lessons · 20 graded challenges · Certificate earned 2026-11-16 ([verify](https://coddy.tech/certifications/tj6FfM-cybersecurity-cFAQiR))

[← Back to portfolio summary](README.md)

> **About the commands below:** This is a simulated endpoint investigation at *Lumen Coffee* using **`seclab`**. Coddy grades the final state of the environment and does not keep a copy of what was typed. The commands below are reference solutions I wrote afterward. Host, process, file and startup-entry names come from the lab's final state. Values I had to read from evidence records during each scenario (parent processes, filenames in audit logs) are shown as `<placeholders>`.

## What this course covers

Host-level incident response: identifying the process behind a suspicious connection, following parent processes, verifying files by SHA-256 fingerprint, **preserving evidence before cleanup**, stopping processes, quarantining files, disabling startup persistence, testing through restarts and retries, building a timeline, separating confirmed data loss from blocked attempts, finding renamed copies by hash, restoring a wrongly quarantined service, and returning an isolated machine to service.

### `seclab` commands used

| Purpose | Command |
| --- | --- |
| List/read processes | `seclab inspect processes [NAME]` |
| Read a connection | `seclab inspect connections NAME` |
| Read a file + SHA-256 | `seclab inspect artifacts NAME` |
| Read startup entries | `seclab inspect startups NAME` |
| Check operations (checkout, backup) | `seclab inspect operations NAME` |
| Snapshot evidence / confirm it | `seclab preserve HOST` · `seclab inspect evidence` |
| Stop / start a process | `seclab process NAME stop\|start` |
| Quarantine / restore a file | `seclab artifact NAME quarantine\|restore` |
| Disable a startup entry | `seclab startup NAME disable` |
| Restart / reconnect a host | `seclab host HOST restart\|reconnect` |
| Advance one interval / read logs | `seclab wait` · `seclab logs` |
| Record a finding | `seclab answer NAME VALUE` |

## Contents

1. [Follow the Evidence](#1-follow-the-evidence)
2. [Make the Cleanup Last](#2-make-the-cleanup-last)
3. [Work Out What Happened](#3-work-out-what-happened)
4. [Prove the Recovery](#4-prove-the-recovery)
5. [Incident summary](#incident-summary)

---

## 1. Follow the Evidence

### 1.1 Running Programs · Easy
**Ticket:** design-laptop is talking to `parcel-cache`. Which process opened that connection?
```bash
seclab inspect processes
seclab inspect connections <flagged-connection>    # names the process
seclab inspect processes desk-helper
seclab answer process desk-helper
```
Leave everything running at this stage, because evidence comes first.

### 1.2 Parent Processes · Easy
**Ticket:** What launched the helper?
```bash
seclab inspect processes desk-helper               # "Parent process: ..."
seclab inspect processes <parent>
seclab answer parent <parent>
```
An approved parent (such as a browser) can still launch an unwanted child, so check the child before blaming the parent.

### 1.3 File Fingerprints · Easy
**Ticket:** Compare the helper's file with the approved reference.
```bash
seclab inspect artifacts desk-helper               # SHA-256 vs Expected SHA-256
seclab answer file desk-helper
```
A SHA-256 mismatch means the contents differ from what was approved.

### 1.4 Save the Evidence · Easy
**Ticket:** Preserve the original process and connection records before any cleanup.
```bash
seclab preserve design-laptop
seclab inspect evidence                            # confirm the snapshot exists
```
The snapshot only captures what's running *now*, which is why you preserve before you stop anything.

### 1.5 Another Report · Medium
**Ticket:** support-laptop is showing the same pattern.
```bash
seclab inspect processes
seclab inspect connections <outside-connection>
seclab inspect processes note-sync
seclab answer process note-sync
seclab answer parent <parent>
seclab inspect artifacts note-sync
seclab answer file note-sync
seclab preserve support-laptop
```

---

## 2. Make the Cleanup Last

### 2.1 Stop the Program · Easy
```bash
seclab process desk-helper stop
seclab process note-sync stop
```
Stop only the processes the evidence supports. The backups and checkout keep running.

### 2.2 Quarantine the File · Easy
```bash
seclab artifact desk-helper quarantine
seclab artifact note-sync quarantine
```
Stopping a process and quarantining its file are separate steps. Quarantine prevents a relaunch and keeps the file as evidence.

### 2.3 Startup Entries · Easy
```bash
seclab inspect startups login-cache
seclab startup login-cache disable        # design-laptop → desk-helper
seclab inspect startups session-notes
seclab startup session-notes disable      # support-laptop → note-sync
```
The browser and backup startup entries stay enabled.

### 2.4 Test the Restart · Easy
```bash
seclab host support-laptop restart
seclab wait                               # let the next connection attempt happen
seclab logs                               # expect a blocked attempt, no transfer
seclab inspect operations <backup-op>     # Working: yes
```

### 2.5 The Returning Program · Medium (full cycle on roastery-pc)
```bash
seclab inspect processes; seclab inspect connections <conn>
seclab preserve roastery-pc               # 1. evidence while running
seclab process label-worker stop          # 2. stop
seclab artifact label-worker quarantine   # 3. quarantine
seclab startup label-refresh disable      # 4. remove persistence
seclab host roastery-pc restart           # 5. test
seclab wait; seclab logs
```

---

## 3. Work Out What Happened

### 3.1 Build a Timeline · Easy
```bash
seclab inspect evidence; seclab logs
seclab answer first <parent-that-launched-label-worker>
```

### 3.2 Did Data Leave? · Easy
```bash
seclab logs                               # DATA_TRANSFERRED vs CONNECTION_BLOCKED
seclab answer transferred <filename-from-DATA_TRANSFERRED>
seclab answer blocked <filename-from-CONNECTION_BLOCKED>
```
Only `DATA_TRANSFERRED` confirms a loss. A blocked attempt is not a breach of that file.

### 3.3 Find Another Copy · Easy
**Ticket:** The helper was copied to `label-server` under a different name.
```bash
seclab inspect artifacts print-cache      # same SHA-256 as label-worker
seclab answer copy print-cache
seclab process print-cache stop
seclab artifact print-cache quarantine
seclab startup print-refresh disable
```
Hash matching finds identical contents regardless of the filename.

### 3.4 Restore a Safe Program · Easy
**Ticket:** `checkout-service` was quarantined by mistake.
```bash
seclab inspect artifacts checkout-service # fingerprint matches approved reference
seclab artifact checkout-service restore
seclab process checkout-service start     # restore does not auto-start
seclab inspect operations <checkout-op>   # Working: yes
```

### 3.5 The Contractor's Report · Medium (studio-laptop)
```bash
seclab inspect connections <conn>; seclab inspect processes colour-sync
seclab preserve studio-laptop
seclab process colour-sync stop
seclab artifact colour-sync quarantine
seclab startup colour-profile disable
```

---

## 4. Prove the Recovery

### 4.1 Check Normal Work · Easy
```bash
seclab process checkout-service start
seclab inspect operations <checkout-op>
seclab inspect operations <studio-backup-op>
seclab inspect processes colour-sync      # Running: no
```

### 4.2 Watch for a Return · Easy
```bash
seclab wait; seclab logs                  # repeat until the retry appears
# confirm CONNECTION_BLOCKED and no new DATA_TRANSFERRED
```
Watch a retry happen before claiming the protection works.

### 4.3 Write the Findings · Easy
```bash
seclab answer host studio-laptop
seclab answer cause <parent-that-launched-colour-sync>
seclab answer file colour-sync
```

### 4.4 Return to Service · Easy
```bash
seclab host studio-laptop reconnect
seclab inspect operations <studio-backup-op>   # Working: yes
seclab inspect processes colour-sync           # still stopped
```

### 4.5 The Stock Room · Medium (capstone, warehouse-pc)
```bash
seclab inspect processes; seclab inspect connections <conn>
seclab preserve warehouse-pc
seclab process stock-helper stop
seclab artifact stock-helper quarantine
seclab startup stock-launch disable
seclab host warehouse-pc reconnect
seclab inspect operations <warehouse-backup-op>
seclab host warehouse-pc restart; seclab wait; seclab logs
seclab inspect operations <checkout-op>
```

---

## Incident summary

| Host | Unwanted process / file | Startup persistence | Outcome |
| --- | --- | --- | --- |
| design-laptop | `desk-helper` | `login-cache` | Preserved, stopped, quarantined, disabled |
| support-laptop | `note-sync` | `session-notes` | Preserved, stopped, quarantined, disabled; restart tested |
| roastery-pc | `label-worker` | `label-refresh` | Full cycle plus timeline and data-loss findings |
| label-server | `print-cache` (renamed copy, matched by hash) | `print-refresh` | Stopped, quarantined, disabled |
| studio-laptop | `colour-sync` | `colour-profile` | Cleaned, watched for retry, returned to service |
| warehouse-pc | `stock-helper` | `stock-launch` | Capstone: full cycle, reconnected, verified |

**The method:** identify the process, trace its parent, check the hash, **preserve**, stop, quarantine, kill persistence, test with a restart and a retry, report facts only, and restore verified-safe services. This is the same workflow used for an endpoint investigation in **Microsoft Defender for Endpoint** (process tree, file hash, live response, isolate/release device).
