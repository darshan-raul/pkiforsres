# Lab 3.2: Man-in-the-Middle (MITM) Simulation

**Goal:** See what happens when someone sits in the middle.

*Note: Visualizing this is hard without a second machine, but we can simulate the concept.*

The "Remote Host Identification Has Changed" error from Lab 3.1 **IS** what a MITM looks like. Detailed tooling like `ssh-mitm` exists, but for SREs, recognizing the warning signature is key.

**Scenario:**
1.  You normally connect to `production-db` (Key A).
2.  Attacker arp-spoofs and directs your traffic to their laptop (Key B).
3.  You SSH to `production-db`.
4.  SSH Client checks `known_hosts`, sees Key A.
5.  Server presents Key B.
6.  **BLOCK.**

**Critical Rule:** Never blindly run `ssh-keygen -R` unless you *know* why the key changed (e.g., "Oh yeah, I rebuilt that server 5 minutes ago").
