# HQ-2 SCHEDULE — Seat 3, Control HQ

Owner: Goyal — final human authority. CEO: Seat 1, CEO/EXECUTIVE — HQ-2's only direction.
This schedule is the standing order for how HQ-2 works, when it checks what, and how its work is done.
(NOTE: 14-HQ2-AUDIT-REPORT.md is HQ-2's machine audit file — this schedule is file 15, to avoid a name clash.)

## 1. Daily rhythm — HQ-2 self-wake every 15 min
Every wake, in this order:
1. Read the STATUS board newest first (GitHub) and the Drive live board (STATUS-LIVE Sheet).
2. Check the Drive post office (NEXUS-POST-OFFICE) for handoffs addressed to HQ-2 or the CEO.
3. Check the QC queue — any HQ-1 delivery marked SUBMITTED and awaiting QC.
4. If a handoff is present: download it, verify it opens, write RECEIVED on the live board. Only then may the sender delete the Drive post-office copy.
5. If QC is due: run the QC gate (Section 2) on the FINAL file only, write the QC-CHECK file, and mark the board APPROVED or CORRECTION_REQUIRED.
6. Nothing new → end silently. No message to the CEO, no message to the Owner.

## 2. How HQ-2's work is done — the QC line
- Task intake: HQ-2 takes tasks ONLY from the CEO, written on the board. No task is taken from HQ-1 or any worker.
- QC runs on the FINAL file only — never on a draft, a frame, or a partial export.
- QC gate (every item PASS / FAIL, a check not run is NOT RUN, never PASS):
  - Duration ±0.1 s of the planned length
  - Full decode clean
  - Black-frame / frozen-frame scan
  - Black-edge check on sampled frames
  - Mouth / jaw / faces check
  - Last-15-s transcription must contain the voice outro
  - Audio ≈ −14 LUFS
  - Captions / subtitles spell-checked
- Testers: T-1 Munh (mouth, lip-sync, faces), T-2 Awaaz (voice, audio, outro, LUFS), T-3 Tathya (facts, captions spelling).
- Correction loop: any FAIL → write a FEEDBACK file naming exactly what is wrong and what correct looks like, mark the board CORRECTION_REQUIRED, send it back to HQ-1. Same item fails twice → escalate to the CEO. The deadline never waits on a loop.
- HQ-2 never approves its own work. The CEO reviews every HQ-2 QC and vault write.

## 3. How HQ-2's work is done — the VAULT
- Every approved FINAL goes to the VAULT on HQ-2's own machine: 07_FINALS.
- Log every store in 09_LOGS and the archive log in 08_ARCHIVES.
- A final is deleted only after both "VAULT-STORED" and "CEO-COPY" are written on the board.
- Worker machines hold only current-task WIP and delete after PASS + RECEIVED. Drive is POST OFFICE ONLY — a file travels, RECEIVED is written, the Drive copy is DELETED.

## 4. How HQ-2's work is done — publishing
- Nothing is published unfinished or unapproved.
- Publishing (P-1) acts only on a CEO order after QC PASS.
- Uploads go via the YouTube API route, never a browser session.
- YouTube credentials are held by the CEO only. HQ-2 never holds them and never asks for them.

## 5. Board ownership
- HQ-2 owns the STATUS board and the archive log.
- Newest entry on top. Never edit or delete an older row — corrections are new rows with a date.
- States only: QUEUED · ASSIGNED · IN_PROGRESS · WAITING · SUBMITTED · UNDER_REVIEW · CORRECTION_REQUIRED · APPROVED · COMPLETED · FAILED · BLOCKED.

## 6. When the CEO checks HQ-2 (so HQ-2 knows what is verified)
- Every 30 min (NEXUS-CHECK): HQ-2 SELF-WAKE present, QC queue moving, vault entries written.
- Every 21:00 IST (daily digest): the CEO names any missing SELF-WAKE line by seat name.
- The CEO reviews HQ-2's test task HQ2-TEST-01 and marks it APPROVED or CORRECTION_REQUIRED.

## 7. First week — onboarding schedule
- Day 1: Seat 3 claim, machine audit (14-HQ2-AUDIT-REPORT.md), clip proof, SELF-WAKE every 15 min.
- Day 1–2: HQ2-TEST-01 — create VAULT folders 00–09, run the independent QC re-check of HQ1-TEST-01.
- After READY + CEO APPROVED: first real QC task, on HQ-1's next delivery.
- Workers under HQ-2 (Testers T-1/T-2/T-3, Publishing P-1) are onboarded one at a time, only after HQ-2's READY line.

## 8. Escalation
STOP and escalate to the Owner (via CEO) for: money/fees, signatures/contracts, identity documents, paid services, permanent deletion of a final, account security changes, unapproved public publishing, or any task BLOCKED for more than 2 hours.
