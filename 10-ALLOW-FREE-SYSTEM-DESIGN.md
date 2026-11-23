# NEXUS — ALLOW-FREE SYSTEM DESIGN (Detail Design, 23 Nov 2026)

Owner: Goyal. CEO: Seat 1, CEO/EXECUTIVE (this account).
Status: DESIGN ONLY — abhi implement nahi hua hai. Owner ki approval ke baad hi banega.
Repo: goyal1024/nexus-studio, branch main. Last verified board commit: b0ef2306.

## 1. Problem — kya atak raha hai

- GitHub par always-allow possible nahi hai. Verified 23 Nov 2026:
  - GitHub grant for NEXUS-CHECK: "Change GitHub repository content" — mode Ask, durable.
  - Connector doc: har GitHub write ke liye fresh approval mangta hai.
  - Google Drive grants for NEXUS-CHECK: Create/upload, Update shared files, Manage sharing — mode Allow.
- Iska matlab: NEXUS-CHECK har 30 minute chalega, board padhega, lekin board par likhte hi approval card par ruk jayega. Raat ko ya Owner ke busy hone par board update atak jayega.
- Drive-only system iska hal nahi hai. Drive mein GitHub jaisi commit history aur SHA verification nahi milti. Aaj seat conflict sirf GitHub commit history se pakda gaya tha (ab4d2b9a, 27483bf7). Drive mein woh proof nahi milta, aur ek galti se delete hui file permanent memory ko mita deti hai.

## 2. Principle — ek hi golden rule

**Cron kabhi GitHub par nahi likhega.**

- Cron sirf padhega: GitHub STATUS board (newest first), TASKS files, Drive post office.
- Cron likhega sirf wahan jahan Allow hai: Google Drive aur apni local draft file.
- GitHub par likhega sirf ek hi kaam, ek hi time par: raat 21:00 IST ka digest + din bhar ke updates ka ek bulk push, Owner ki ek approval se.
- Isse approval cards din mein 48 se gir kar 1 ho jayenge, aur koi run beech mein block nahi hoga.

## 3. Architecture — teen jagah, teen kaam

| Jagah | Kaam | Kaun likhega | Approval |
|---|---|---|---|
| Google Drive — NEXUS-LIVE | Live board, roz ka kaam | Har seat, apne rhythm par, Drive Allow se | Koi approval nahi |
| Local workspace — ~/workspace/nexus/ | Draft + safety copy, har update pehle yahan save hoga | CEO account (NEXUS-CHECK run) | Koi approval nahi |
| GitHub — goyal1024/nexus-studio | Permanent text memory, nightly archive | Sirf CEO, sirf 21:00 push mein | Owner ki 1 approval, roz |

Kuch bhi delete nahi hoga GitHub se. Drive live board se bhi kuch delete nahi hoga — sirf nayi row add hogi. Post office ka purana rule waisa hi rahega: RECEIVED ke baad hi Drive copy delete hogi.

## 4. Drive live board — NEXUS-LIVE

Drive mein ye structure banega (Owner ke Drive mein, CEO account se):

```
NEXUS-LIVE/
  STATUS-LIVE (Google Sheet)
  NAME-REGISTRY-LIVE (Google Sheet)
  PENDING-UPDATES/            (har seat ki pending row ka text file, agar Sheet na khule)
NEXUS-POST-OFFICE/
  (handoff files — purana rule: RECEIVED ke baad delete)
```

STATUS-LIVE Sheet ke columns, har row ek board entry hai:

| Date | Time IST | Seat | Agent | Task | State | Note |
|---|---|---|---|---|---|---|
| 2026-11-23 | 23:31 | 2 HQ-1 | HQ-1 | HQ1-TEST-01 | SUBMITTED | Drive link + awaiting CEO review |

Rules:
- Sirf nayi row add karo, purani row ko edit ya delete mat karo. Newest row sabse upar.
- States wahi honge jo Master Prompt mein hain: QUEUED, ASSIGNED, IN_PROGRESS, WAITING, SUBMITTED, UNDER_REVIEW, CORRECTION_REQUIRED, APPROVED, COMPLETED, FAILED, BLOCKED.
- Ek time par ek hi seat likhegi — apni row add karte time kisi aur ki row ko haath mat lagao. Isse overwrite conflict nahi hoga.
- SELF-WAKE line bhi yahi likhi jayegi: "SELF-WAKE: CONFIGURED AND VERIFIED — every <N> min".

NAME-REGISTRY-LIVE Sheet ke columns: # | Seat | Agent / Account | Claimed date | Status. Seat claim sirf ek baar likha jayega, dobara claim karne par SEAT CONFLICT row likho aur ruk jao.

## 5. Cron rules — har seat ke liye

### CEO — NEXUS-CHECK (already bana hua hai, badalna hai)
- ID: nexus-check. Har 30 minute. Timezone Asia/Kolkata. Next run verified: ~00:01 IST, 24 Nov 2026.
- Naya kaam:
  1. GitHub STATUS board padho (newest first) — sirf padho, likho mat.
  2. Drive NEXUS-LIVE aur NEXUS-POST-OFFICE check karo.
  3. HQ-1 / HQ-2 ki progress dekho, approvals aur upload orders ka faisla karo.
  4. Faisla likho: (a) Drive STATUS-LIVE mein nayi row, (b) local file `pending-board-updates.md` mein nayi entry, timestamp ke saath.
  5. Kuch naya na ho to chup-chaap band ho jao.
  6. GitHub par is run mein kuch mat likho — chahe kitna bhi urgent ho, sirf §9 item ho to alag se Owner ko batao, board push phir bhi 21:00 wale mein jayega.
- Is run mein koi GitHub approval card nahi aayega, kyunki koi GitHub write hi nahi hai.

### Daily digest + GitHub push — NEXUS-DAILY-DIGEST (already bana hua hai)
- ID: nexus-daily-digest. Roz 21:00 IST. Timezone Asia/Kolkata. Next run verified: 21:00 IST, 24 Nov 2026.
- Kaam:
  1. Din bhar ki `pending-board-updates.md` entries ko padho.
  2. GitHub STATUS.md aur NAME-REGISTRY.md ko Drive live board se merge karo — Drive ki nayi rows upar, purani GitHub rows neeche, kuch delete mat karo.
  3. Digest likho: aaj kya hua / waiting / blocked / kal ka plan, aur kaun si seat ki SELF-WAKE line missing hai, naam ke saath.
  4. Ye merged board aur digest Owner ko chat mein dikhao, ek approval card ke saath.
  5. Owner approve kare to hi GitHub push karo. Approve na kare to push kal tak ruk jayega, lekin Drive live board aur local draft mein kuch lost nahi hoga.

### Baaki seats — har seat apna self-wake khud banayegi
| Seat | Rhythm | Kaam |
|---|---|---|
| HQ-1, HQ-2 | har 15 minute | GitHub padho, Drive live board likho, apna IN_PROGRESS task ek step badhao |
| Busy worker | har 20 minute | Wahi, plus kaam shuru karne se pehle aur deliver karne ke baad turant check |
| Idle worker | har 45 minute | QUEUED task apni seat ke scope mein ho to claim karo |
| Publishing P-1 | har 15 minute, jab APPROVED final waiting ho | Upload tabhi jab QC PASS + CEO FINAL APPROVAL + CEO UPLOAD ORDER, teeno Drive live board par hon |

Har seat ka self-wake bhi GitHub par nahi likhega — sirf Drive live board aur local draft. CEO sirf board par verify karega ki sabne SELF-WAKE line likhi hai ya nahi.

## 6. Approval flow — din mein kitni baar Owner ko bulaya jayega

- Roz: 1 baar, 21:00 digest + GitHub push approval.
- Urgent: sirf §9 wali cheezein — paise/fees, contract/sign, identity document, paid service, final ka permanent delete, account security change, plan ke bahar publish, ya 2 ghante se zyada BLOCKED kaam.
- Uske alawa: Owner ko kuch nahi bheja jayega. Silence from Owner is never a stop signal — Drive live board aur cron apna kaam karte rahenge.

## 7. Kya nahi badlega

- Vault: HQ-2 ki machine par hi rahega, folders 00_CEO_CONTROL se 09_LOGS tak. Drive vault nahi hai.
- Media: GitHub par kabhi nahi jayega. Media sirf Drive post office se jayega, RECEIVED ke baad delete hoga.
- QC gate: FINAL file par hi, duration ±0.1s, last-15s transcription mein voice outro, lagbhag −14 LUFS, clean decode, black/frozen frame, black-edge, mouth/jaw/faces, captions spell-check.
- Voice outro: END par hi, Owner ki line "Subscribe to kar lo yaar!" lagbhag 1.7s, Hindi audio, English subtitles hamesha, no profanity, family-safe.
- YouTube: credentials sirf CEO ke paas, kisi aur seat ko kabhi nahi. Upload sirf CEO order par, QC PASS ke baad, YouTube API route se, browser session se nahi.
- Builders never approve their own work. HQ-2 ki line HQ-1 ki line ko check karegi.
- Onboarding: ek-ek karke, seat order mein, har seat ki READY line ke baad hi agla. HQ-1 aur HQ-2 pehle, phir workers.

## 8. Risks aur unka hal

| Risk | Hal |
|---|---|
| Do seats ek hi time par Drive Sheet likhen | Sirf append-only row, kisi ki row edit mat karo; agar Sheet lock ho to PENDING-UPDATES text file mein likho, CEO merge karega |
| Drive file galti se delete ho jaye | NEXUS-LIVE folder kabhi delete mat karo; GitHub nightly archive backup hai; local draft bhi backup hai |
| GitHub aur Drive live board mein farak aa jaye | Newest entry wins. 21:00 merge mein Drive ki rows upar, GitHub ki purani rows neeche. Conflict ho to board par BLOCKED likho, guess mat karo |
| Owner 21:00 approval na de paye | Push kal tak ruk jayega, kaam nahi rukega — Drive live board hi sach hai us waqt tak |
| HQ-1 ka HQ1-TEST-01 review atak jaye | Woh Drive aur GitHub dono par SUBMITTED hai, awaiting CEO review. CEO review Drive file ko padh kar karega, GitHub push ka wait nahi karega |

## 9. Rollout — approval ke baad, order mein

1. NEXUS-CHECK cron ka body badlo — write-free mode, Drive + local draft. Verify karo: cron.view se body aur schedule check karo.
2. Drive mein NEXUS-LIVE aur NEXUS-POST-OFFICE folders banao, STATUS-LIVE aur NAME-REGISTRY-LIVE Sheets banao, current GitHub board se shuru karo.
3. Local file `pending-board-updates.md` banao, khaali se shuru karo.
4. Daily digest cron ka body badlo — 21:00 merge + push + approval.
5. Ek test cycle chalao: ek dummy row Drive live board mein likho, pending file mein dekho, 21:00 push mein GitHub par aayi ya nahi verify karo.
6. HQ-1 aur HQ-2 ko ye design do — unke self-wake bhi isi rule par banenge. Unki SELF-WAKE line Drive live board par verify karo, raat ke digest mein naam ke saath batao.
7. Is design doc ko GitHub par bhi push karo, jab Owner approve kare.

## 10. Decision needed — Owner se

- Kya ye design approve hai? Haan to main rollout shuru karun (step 1 se).
- Ya pehle Drive live board ka naam / Sheet ka format badalna hai?
- GitHub push ka time 21:00 IST hi rahe ya koi aur time ho?

---
Design prepared by CEO, Seat 1, 23 Nov 2026. Master Prompt aur Scheduling Addendum ke saath standing order hai — jahan design aur Master Prompt takraye, Master Prompt ki storage law (§3), QC gate (§6) aur escalation list (§9) jeetegi. Owner ki boli hui baat sabse upar hai.
