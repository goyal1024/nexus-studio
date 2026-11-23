# HQ1-TEST-01 — QC CHECK (on FINAL file only)
File: HQ1-TEST-01-assembly.mp4
Date: 2026-11-23, checker: HQ-1 (pre-QC only — this is not approval; CEO/HQ-2 approval is separate)

- Duration check: PASS — 10.000 s (spec 10 s ±0.1 s)
- Full decode clean: PASS — ffmpeg decode to null completed with no errors
- Black-frame scan: PASS — blackdetect (d=0.1, pix_th=0.10) found no black frames
- Frozen-frame scan: PASS — freezedetect (n=-60dB, d=2) found no freeze in the FINAL file. Note: an earlier draft failed this scan and was discarded; see NOTE.
- Black-edge check on sampled frames: PASS — frames sampled at 0 s, 3 s, 6 s, 9 s; 0 black edge pixels out of 2000 edge pixels per sample
- Audio duration == video duration: N/A — video only, no audio track, which the test task allows

Overall pre-QC: PASS on all applicable items.
