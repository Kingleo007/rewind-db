# RewindDB — Ultimate Judge Demo Guide

## 🎭 Context
You will present using the **RewindDB Frontend Dashboard**. 
* **If Offline/Local:** Go to `http://localhost:5500`
* **If Online/Pinggy:** Go to your Vercel URL and configure the Pinggy Tunnel in the ⚙️ Settings.

---

## Step 1: The Blank Slate
**Goal:** Show that the database is completely empty and no state is hardcoded.
* **Action:** Show the Dashboard. Balances are $0.00. Events are 0.
* **Talking Point:** "RewindDB does not store a `users` table or a `balances` table. State does not exist until we calculate it from history."

---

## Step 2: Seed the Event Log (30 seconds)
**Goal:** Create raw events that generate the state.
* **Action:** Click the **Seed Demo Data** button on the Dashboard.
* **Talking Point:** "We just fired a batch of events: `AccountCreated`, `MoneyDeposited`, and `MoneyTransferred`. If we navigate to the **Timeline** tab, you can see the raw, immutable ledger."

---

## Step 3: Replay Engine & Determinism
**Goal:** Prove that the state is re-calculated in real-time.
* **Action:** Go to the **Replay Engine** tab. Click **Run Full Replay**.
* **Talking Point:** "Notice the duration in milliseconds. The system just read the entire history log and deterministically folded the events to recreate the exact account balances."

---

## Step 4: The Failure Lab — CRASH
**Goal:** Prove that a system crash means nothing to an Event-Sourced system.
* **Action:** Go to the **Failure Lab** tab. Click **Crash System**.
* **Talking Point:** "We just deleted the in-memory state. In a traditional database, if the disk corrupted without a backup, data is gone. In RewindDB, simply go back to the Replay Engine and hit Replay. State is instantly restored pixel-perfect."

---

## Step 5: The Failure Lab — IDEMPOTENCY
**Goal:** Prove that duplicate processing bugs can't steal money.
* **Action:** In the **Failure Lab**, click **Inject Duplicate Event**.
* **Talking Point:** "We just tried to process the exact same $100 deposit event twice. Our Projector's idempotency guard caught the exact `event_id` and skipped it. The math is protected."

---

## Step 6: The Failure Lab — CORRUPTION
**Goal:** Prove that malicious memory modification is overwritten.
* **Action:** In the **Failure Lab**, click **Corrupt Memory**.
* **Talking Point:** "We injected a garbage balance of $11,649.99 into RAM. But because the event log is the only Source of Truth, the next time the state validates or replays, it immediately corrects the balance back to where it should be."

---

## Step 7: The Failure Lab — CONCURRENCY (Race Conditions)
**Goal:** Prove that concurrent threads can't violate rules.
* **Action:** In the **Failure Lab**, click **Concurrent Writes**.
* **Talking Point:** "We unleashed 5 parallel threads trying to manipulate the same account simultaneously. EventStoreDB's Optimistic Concurrency mechanism threw `WrongExpectedVersion` errors, allowing only the mathematically valid events to append. No race conditions."

---

## Step 8: State Validation
**Goal:** Show post-replay invariant checks.
* **Action:** Go to the **State Validation** tab and click **Run Validation**.
* **Talking Point:** "Our validation engine scans the projected state mathematically. It proves there are no negative balances, zero orphaned events, and total system consistency."

---
*End of Demo.*
