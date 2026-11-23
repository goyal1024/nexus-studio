# PRODUCTION POLICY — NEXUS

Studio: AI animation + YouTube production. Professional 2D / 2.5D animated videos — episodes first, then specials, then full films.

## Pipeline — exact order, every production
CEO episode brief → Research (facts + source URLs) → Script → Storyboard / shot plan → Voice recording + VOICE GATE (every line verified by transcription BEFORE any animation — a failed line is never animated around) → Timeline / keys → Animation (Blender states) → Compositor frames → Inserts → Sound mix (music energy changes per section; silence only before a twist) → Captions from the FINAL mix (+ English subtitles) → QC GATE → CEO final review → Publishing → Vault.

## Animation setup
- Two gears, one asset base. 2D (flat): parts and plates composited directly — fast, for Shorts and simple scenes. 2.5D (depth): the same 2D parts stand as planes in Blender over 3-layer backgrounds (far/mid/near) with camera moves = parallax depth — for episodes and films.
- Tools (all free): Blender · Inkscape / GIMP / Krita (parts & plates) · Python + PIL compositor (frames) · ffmpeg (assembly, mix, captions, master).
- Do NOT install server stacks (no n8n / Celery / PostgreSQL / Prometheus / Grafana) — the board system needs none of it.
- Character system: part-puppet — separate transparent PNGs (body, arms, head, eyes open/closed, mouth shapes, jaw, hair) on ONE shared canvas, with a MANIFEST of pivots and a full-face expression set (neutral / happy / angry / surprised / sad / thinking). Master art lives in the vault; parts are derived from it. Animators animate ONLY from vault-approved packs — nobody redesigns a character without CEO approval.
- Stage system: 3-layer background plates per location · 7 ready camera presets · ONE colour grade per video · small living details (lamp, clock, window light).

## QC GATE — on the FINAL file, never on intermediates
Audio duration == video duration (±0.1 s) · transcription of the last 15 s must contain the voice outro · loudness ≈ −14 LUFS · full decode clean · black-frame / frozen-frame scan · black-edge check on sampled frames · mouth placement, jaw, faces spot-read · captions spell-checked against the script.
FAIL on any item = no delivery, no upload.

## Voice outro (every video)
The Owner's real voice line "Subscribe तो कर लो यार!" (≈1.7 s) at the END only, never the start — blended: music ducks and continues under the voice, picture keeps moving, then music + picture fade out together (~0.6 s).

## Content rules
Hindi audio; English subtitles always; no profanity; family-safe surface; facts carry sources; titles = search phrase + curiosity hook; a beat every 35–80 s; no static frame over ~5 s under dialogue; listeners react — frozen listeners are a defect.

## Scope ladder
Shorts → 10-min episodes → 20–30-min specials → 60–90-min films.
Films additionally require: the Story Bible (GitHub — characters, places, timeline, looks; every shot must match it), a named Continuity Tester duty, and scene-wise production (scenes bank into the vault as they are approved, never one giant end-stage assembly).

## Rendering + compute
Default rendering = each animator's own machine (PIL compositor route). Oracle Cloud (Owner's free tenancy, CPU only — never assume a GPU) may be used for heavy bursts ONLY within the free tier. Paid compute of any kind requires Owner approval first. Never promise render times; report measured performance.

## Storage law (summary)
Worker machines hold only their CURRENT task's work-in-progress. Task PASS + handoff received → local WIP is deleted.
HQ-1 holds the running episode's production copy. Episode final → it clears to the vault.
HQ-2's machine = THE VAULT — the team's only permanent store.
CEO's machine holds ONE safety copy of FINALS only.
GOOGLE DRIVE = POST OFFICE ONLY. A file enters Drive only to travel from one agent to another. Receiver downloads it and writes RECEIVED on the board → the Drive copy is DELETED. Drive stores nothing long-term.
GITHUB = PERMANENT TEXT MEMORY. STATUS board, rules, charters, prompts, scripts, story bible, archive log — all text lives here. Media NEVER goes to GitHub.
DELETE RULES: nothing is deleted before its RECEIVED confirmation exists. A final is deleted from anywhere only after BOTH "VAULT-STORED" and "CEO-COPY" confirmations are on the board. Never delete the only copy of anything.
