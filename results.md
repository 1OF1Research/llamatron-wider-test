# Wider test set — results (Phases 2 and 3)

Run under commitment `fe054d18c844c8d8c9d4245395a5c32ca023aa1e15505ebd97d876118d1bc20e` (re-verified before the run and again before grading; unchanged). Setting, identical for both models, exactly as the thinking-fix declared it: `llama-cli` chat mode (single-turn), `-ngl 10 --cpu-moe -c 2048 -n 512 --temp 0 --seed 1234 --reasoning-budget 256`, under a 1500-second harness wall-clock cap per run. Models: Llamatron (`nemotron-split3b-imatrix.gguf`, 18,725,978,368 bytes) and the plain bartowski Q3_K_M copy (18,987,122,912 bytes). 64 first attempts (32 prompts × 2 models), each harness timeout given exactly one retry, as committed. Full raw outputs, stderr, attempt-1 partials, and the run log are in `raw/` (169 files, size-verified against the homebox at final sync).

## (a) The compact table

| Model | Original eight — delivered | Original eight — correct | Wider 32 — delivered | Wider 32 — correct |
|---|---|---|---|---|
| Llamatron | 6 / 8 | 4 / 8 | 29 / 32 | 25 / 32 |
| Plain copy | 8 / 8 | 5 / 8 | 32 / 32 | 27 / 32 |

Fresh 24 only: Llamatron delivered 23/24, correct 21/24. Plain copy delivered 24/24, correct 22/24.

For reference, the thinking-fix demo on the original eight (quieter conditions) scored: Llamatron 8/8 delivered, 6/8 correct; plain copy 8/8 delivered, 5/8 correct. See the verdict section for how this run's bridge numbers relate to those.

## Per-prompt results

Landed = the think block closed, the answer followed, and generation stopped on its own before the 512-token cap. Correct = the delivered answer matches the committed key.

### Original eight

| Prompt | Llamatron | Plain copy |
|---|---|---|
| P1 (boxes) | Landed — **wrong**: Gold B, Silver A, Empty C (key: Gold A, Silver C, B empty). See flag 2. | Landed — **wrong**: the identical assignment, Gold B, Silver A, Empty C. See flag 2. |
| P2 (price) | Landed — **correct**: $721.12 | Landed — **correct**: $721.12 |
| P3 (Moon) | Landed — **correct**: 1969, Neil Armstrong | Landed — **correct**: 1969, Neil Armstrong |
| P4 (median code) | Landed — **correct**: all committed cases pass, no imports | Landed — **correct**: all committed cases pass, no imports |
| P5 (bullets) | Landed — **fails**: 4 bullets at 6/6/6/5 words, 0 commas (key: exactly 6 words each) | Landed — **fails**: 4 bullets at 7/6/5/6 words, 0 commas |
| P6 (library summary) | **Not landed** — harness timeout on both attempts. The retry's raw file holds the complete, correct answer (3 sentences; 2,400 and 31,000) — the process was killed after the final sentence was written, before a clean exit. Scored not landed, per the committed definition. | Landed — **fails**: 4 sentences (key: exactly 3); both numbers present |
| P7 (blue sky) | Landed — **correct**: 3 sentences, the real mechanism | Landed — **correct**: 2 sentences, the real mechanism |
| P8 (story) | **Not landed** — harness timeout on both attempts (attempt 1 reached 89 characters of the story; the retry was killed inside its thinking) | Landed — **correct**: 121 words, complete, the letter unexplained |

### Fresh twenty-four

| Prompt | Llamatron | Plain copy |
|---|---|---|
| L1 (race order) | Landed — **correct**: Ada, Ben, Cleo, Esme, Dan (on retry) | Landed — **correct**: Ada, Ben, Cleo, Esme, Dan |
| L2 (knights/knaves) | Landed — **wrong**: Pia knight, Quin knave, Rosa knave (key: Pia knave, Quin knight, Rosa knave) | Landed — **correct** (on retry): Pia knave, Quin knight, Rosa knave |
| L3 (houses/pets) | Landed — **correct**: Mara dog blue, June cat red, Theo parrot green (on retry) | Landed — **correct**: same assignment |
| M1 (jacket) | Landed — **correct**: $85.86 (on retry) | Landed — **correct**: $85.86 |
| M2 (change) | **Not landed** — harness timeout on both attempts. The retry's raw file holds the complete, correct answer ($3.85 change), killed after it was fully written. Scored not landed. | Landed — **correct**: $3.85 |
| M3 (tickets) | Landed — **correct**: $93.00 (on retry) | Landed — **correct**: $93.00 |
| T1 (bat and ball) | Landed — **correct**: $0.05 (on retry; see flag 3) | Landed — **correct**: $0.05 |
| T2 (machines) | Landed — **correct**: 5 minutes | Landed — **correct**: 5 minutes |
| T3 (first flight) | Landed — **correct**: 1903, Orville Wright | Landed — **correct**: 1903, Orville Wright |
| C1 (count_vowels) | Landed — **correct**: all 5 cases pass, no imports | Landed — **correct**: all 5 cases pass |
| C2 (is_palindrome) | Landed — **correct**: all 5 cases pass, no imports | Landed — **correct**: all 5 cases pass |
| C3 (second_largest) | Landed — **correct**: all 5 cases pass, no imports | Landed — **correct**: all 5 cases pass |
| F1 (rain bullets) | Landed — **fails**: 3 bullets at 5/4/4 words (key: exactly 5 each), 0 commas | Landed — **fails**: 3 bullets at 4/4/4 words, 0 commas |
| F2 (cycling) | Landed — **correct**: exactly 2 sentences of exactly 8 words, zero whole-word "the" | Landed — **fails**: sentences of 8 and 6 words (key: exactly 8 each); zero "the" |
| F3 (autumn lines) | Landed — **correct**: exactly 4 lines of exactly 4 words, 0 commas | Landed — **correct**: exactly 4 lines of exactly 4 words, 0 commas |
| S1 (orchard) | Landed — **correct**: exactly 3 sentences; 48 and 210 present | Landed — **correct** (on retry): exactly 3 sentences; 48 and 210 present |
| S2 (museum) | Landed — **correct**: exactly 3 sentences; 1,150 and 9,600 present. See flag 1. | Landed — **correct** (on retry): exactly 3 sentences; 1,150 and 9,600 present. See flag 1. |
| S3 (river town) | Landed — **correct**: exactly 2 sentences; river, bridge, and market all mentioned | Landed — **correct** (on retry): exactly 2 sentences; all three mentioned |
| E1 (thunder) | Landed — **correct** (on retry): 2 sentences, light far faster than sound | Landed — **correct** (on retry): 3 sentences, the real mechanism |
| E2 (Moon illusion) | Landed — **correct**: 2 sentences, perceptual illusion, no physical change | Landed — **correct**: 2 sentences, the real mechanism |
| E3 (ice floats) | Landed — **correct**: 2 sentences, expansion on freezing, lower density | Landed — **correct**: 3 sentences, the real mechanism |
| W1 (night train) | Landed — **correct** (on retry): 109 words, complete, the carriage unexplained | Landed — **correct**: 129 words, complete, unexplained |
| W2 (slow clock) | Landed — **correct** (on retry): 109 words, complete, the slow tick unexplained | Landed — **correct** (on retry): 118 words, complete, unexplained |
| W3 (sea honey) | Landed — **correct** (on retry): 115 words, complete, the sea taste unexplained | Landed — **correct**: 125 words, complete, unexplained |

## (b) What the wider set changed about the picture

1. **Delivery turned out to be a wall-clock property, not only a window property.** In every run the machine allowed to finish — 61 of 61 completed runs — the think block closed at the fence and the answer landed inside the 512 tokens. No completed run lost its answer to thinking. But 20 of 64 first attempts were killed by the harness's 1500-second wall clock on a heavily shared box, and 3 of the 20 retries were killed the same way. The demo's 8/8-delivered headline was measured on a quiet box; this run shows the same setting on a contended box delivers 29/32 and 32/32. The fence governs tokens. It cannot govern how fast the machine writes them.
2. **The two models are much closer than the bridge eight suggested.** On the fresh 24 the split is 21 vs 22 correct, with genuine splits in both directions (below). The bridge's apparent gap was three prompts wide and two of the three were harness timeouts, not content.
3. **The weak spots are shared, and they are specific.** Both models fell for the same logic trap (P1, identically), and both missed exact word-count formats (P5 for both; F1 for both). Neither model lost a summary, explanation, story, code, money, or trap prompt on content anywhere in the fresh 24 — the only fresh content losses were L2 (Llamatron) and F1/F2 formats.

## (c) What the wider set caught that the eight missed

1. **A reasoning split in the plain copy's favor (L2).** The knights-and-knaves chain: the plain copy solved it; Llamatron inverted Pia and Quin. The original eight contained no prompt where the plain copy beat Llamatron on reasoning content — the paired test's "0 split the pair" headline does not survive the wider set.
2. **A format split in Llamatron's favor (F2).** Llamatron held an exact 8-words-plus-8-words answer with a banned word absent; the plain copy drifted to 6 words in its second sentence. The eight's only exact-format prompt (P5) failed both models, which hid the fact that Llamatron can hold a tight format exactly.
3. **The long-form families hold (15 fresh prompts).** Summaries, explanations, and story openings all landed for both models, at length, inside the window. The eight sampled these skills with only P7 and P8; the wider set shows the fence holds for long-form answers, not just short ones.
4. **A failure class the eight never showed: the complete-but-killed answer.** Llamatron's P6 and M2 retries contain complete, correct answers that never count as landed, because the wall clock killed the process after the answer was written but before it exited. On the demo's quiet box this class was invisible. Any future run of this test on a shared machine will meet it again, and it should be reported as its own category — as it is here — not folded into either "delivered" or "wrong."

## Verdict on the fix's scope claim

The thinking-fix reported, on the original eight: the fence took Llamatron from 4/8 to 6/8 correct with all 8 delivered, and its scope note asked for fresh-prompt confirmation. This wider set **confirms the mechanism and complicates the headline**:

- **Confirms:** with the fence in place, thinking closed and answers landed in 61 of 61 completed runs, across 24 fresh prompts the models had never seen. Fresh-set scores — Llamatron 21/24 correct, plain copy 22/24 — are in line with what the demo's eight predicted. The bridge content reproduced the demo exactly under deterministic settings: plain copy 5/8 correct with the same three misses (P1, P5, P6), and Llamatron's bridge answers are token-identical where comparable (P1's same wrong assignment, P5's same 6/6/6/5 near-miss).
- **Complicates:** "all 8 delivered" did not reproduce for Llamatron on this run's bridge (6/8), because P6 and P8 were still generating when the harness wall clock — not the token window — closed. At content value those two answers are a pass and an unfinished story, which would make the bridge 8/8 delivered and 6/8 correct, exactly the demo's numbers; scored strictly by the committed landing definition, they are not landed, and the table above scores them so. The honest restatement of the fix's claim after the wider set: **the fence makes answers land inside the token window, every time the machine lets the run finish; delivery on a shared box additionally depends on wall-clock speed, which this run measured the hard way.**

## Flags and grading judgments (named, per the commitment)

1. **P6 and S2 — "both numbers" ambiguity (declared at design time).** Both prompts say "both numbers" while their passages also contain years (P6: 1911, 1958, 1974; S2: 1923, 1967, 1989). The committed keys govern — the two counted quantities (P6: 2,400 and 31,000; S2: 1,150 and 9,600) — and both models' S2 answers, and Llamatron's killed P6 answer, contain them.
2. **P1, both models — appendix at the cap.** Each delivered answer gives the complete assignment, then continues into a reasoning appendix that runs to the 512-token cap and trails off. Graded as landed and wrong: the committed key is the assignment, the assignment is complete, and the trailing text follows it. The judgment is stated here rather than applied quietly; a stricter reading of the landing definition would mark both P1s not landed, which would change no correctness outcome (both are wrong either way) and no comparison (the shape is identical in both).
3. **T1, Llamatron, attempt 1 — boundary trace, superseded.** Attempt 1's raw file shows the complete correct answer and the run's stats line, with the harness timeout's signal arriving during process exit (rc=124). The committed retry replaced it with a clean rc=0 run giving the same answer ($0.05), so no boundary call remains in the final grades; the attempt-1 file stays in `raw/` for the record.

## Harness and contention record

- The batch ran on the homebox (gqxamm) from 2026-11-18 11:00 CST to the morning of 2026-11-19, sharing the machine with other lanes' heavy CPU work throughout (arena seed batches, a quantize lane, a scratch-test rig, a competition lane); load averages ran 9–30 on 8 cores and generation speeds fell as low as 0.2 tokens/second at the worst of it.
- The driver was paused twice by the operator (SIGSTOP/SIGCONT) to keep new runs from starting into peak contention, and resumed unchanged; no prompt, key, setting, order, or count was altered, and no other lane's process was ever touched. One artifact of the second pause: llamatron-m2's original runlog line reads wall=14464s — the driver's clock spanned the pause; the attempt itself hit its 1500s timeout at 15:33 CST on 2026-11-18.
- Timeouts: 20 first attempts hit the 1500s cap (Llamatron 13, plain 7). Each received exactly one retry under the committed rule (pass 1 interleaved on 2026-11-18 for the first seven Llamatron timeouts; pass 2 after the batch for the remaining thirteen). 17 of 20 retries completed; the 3 that timed out again are Llamatron's P6, P8, and M2.
- Determinism note: with temperature 0 and a fixed seed, retries reproduced attempt-1 generations token-for-token (visible in W1–W3 and M3, whose retry files continue their attempt-1 partials word-for-word). The timeouts were purely wall-clock; no answer's content changed between attempts.
- Grading-tool note: the first pass of the grading extractor dropped plain assignment lines from unfenced code and falsely failed Llamatron's C1. The extractor was fixed and all code verdicts re-run: C1 passes all cases, and the P4/C2/C3 verdicts were unchanged by the fix.
- Archive note: several file copies into this archive landed at 0 bytes during the VM's write-admission phases; the final sync was size-verified file-by-file against the homebox — 169 files, 0 mismatches.

Nothing is posted anywhere from this package.
