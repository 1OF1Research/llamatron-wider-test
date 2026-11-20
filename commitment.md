# Wider test set — commitment (written before any generation)

- Written: 2026-11-18, before a single token of this test was generated.
- Why: the owner tapped YES (main chat, 2026-11-18 08:10 CST) to a wider Llamatron test set, run as its own measured piece with the same discipline as the first eight. The first eight prompts (../2026-11-17-paired-test/) all tied — 4 answered correctly by both models, 4 by neither, 0 split — at a 512-token window in plain completion mode. The thinking-fix work (../2026-11-18-thinking-fix/) then built a demo setting that fences thinking so a final answer can land inside the same 512 tokens. This set exercises that fixed demo setting on 32 prompts: the original 8 as the bridge, plus 24 fresh ones.
- Machine: homebox `gqxamm` (hostname asserted by SSH read before the runs), GTX 1650 Max-Q 4 GB, 16 GB RAM.

## Freshness honesty — read this first

The 24 fresh prompts (L1–L3, M1–M3, T1–T3, C1–C3, F1–F3, S1–S3, E1–E3, W1–W3) were written for this set on 2026-11-18. **They have never been run against either model, and nothing in them was tuned against any model output.** They were designed from the eight kinds of the original set only. Every grading key below was fixed in this document before any generation. If a prompt turns out ambiguous in grading, it is flagged by name in results.md — never quietly scored. One design fault was caught and fixed at design time, before this commitment was hashed: an early draft of L3 had contradictory clues (no solution existed); clue (5) was corrected and the puzzle re-solved by hand before inclusion.

## The two models (identical to the paired test and the thinking-fix)

- **Llamatron** — `/home/agent/hf-weekend/llamatron-keep/nemotron-split3b-imatrix.gguf`, 18,725,978,368 bytes, SHA-256 `99d564ee23b82f3fc1cc15bb3a450f5c5694df2f3e39814dfb999835dcd4b8b4`.
- **Plain copy** — `/home/agent/paired-test-20261117/dl/nvidia_Nemotron-3-Nano-30B-A3B-Q3_K_M.gguf` (bartowski quant of the same stock model, NVIDIA Nemotron-3-Nano-30B-A3B), 18,987,122,912 bytes, SHA-256 `1b282e9ddf8622bf318dd9fadcc76cea1bac8815fbbef5bd76581336ba3f27b0`. Same declared substitution as the paired test: the card's own plain-copy file no longer exists on the box, so a fresh standard plain 3-bit quant stands in.

## The setting — the fixed demo setting, gated on the thinking-fix result

- **Gate:** no generation under this commitment begins until `../2026-11-18-thinking-fix/` holds its finished results (`results-demo.md`) declaring the demo default, and no other llama-cli job is active on the box.
- **Setting used:** the demo default the thinking-fix results declare. Expected, per its commitment (`commitment-demo.md`, SHA-256 in its folder): binary `/home/agent/llama-v1/build/bin/llama-cli` (version 1.0.0-dev, build 1, commit b1b770d), command shape `llama-cli -m <model> -ngl 10 --cpu-moe -c 2048 -n 512 --temp 0 --seed 1234 -st -f <prompt file> --reasoning-budget 256`, stdin closed, 1500-second timeout per run. `-st -f` delivers the prompt as a single-turn user message through the model's own chat template; the reasoning-budget sampler fences thinking at 256 tokens and forces the end-of-thinking sequence there, so the answer gets the rest of the same 512-token window. Reasoning + inserted closing tokens + answer all count against the 512.
- If the thinking-fix results declare a different demo default, that declared command replaces the shape above, identically for both models, and the substitution is recorded at the top of results.md. If the thinking-fix declares failure with no working demo setting, this test does not run and that is the report.

## Run rules

- One generation per prompt per model. First output stands. No re-rolls. A harness error (crash, timeout, load failure) is recorded and gets exactly one retry; a weak or wrong answer is never a reason to retry.
- Before every run, the runner waits until no other llama-cli process is active on the box (script-file runner, the run-one.sh pattern).
- Order: Llamatron first — P1–P8, L1–L3, M1–M3, T1–T3, C1–C3, F1–F3, S1–S3, E1–E3, W1–W3 — then the plain copy in the same order. 64 runs total.
- Outputs captured raw and unedited — full stdout and stderr per run under `raw/`, plus wall-clock seconds per run in `raw/runlog.txt`.

## The prompts

**Original eight** — exact files, read in place from `../2026-11-17-paired-test/prompts/`, never modified. SHA-256 verified against the paired commitment on 2026-11-18:
- p1.txt `b049bd528e6a88b14f7adcd99da947e46724d2e1def810100fcc90c07b87476e`
- p2.txt `584642770543eaf20aea6a1efcfae0a9baefc7b6f07f9321dbdde6d2fbd2bf7e`
- p3.txt `476c809adcc66ae9df7216d04500f2450ab90e1dc6bfe53e308422a1d52d3f07`
- p4.txt `97d2f3a828f9503c801c202e71497175b6a0be5966328b0fdacb5341ccbd2649`
- p5.txt `40be4e436d0cf0ba066481a05b4ac38c593bb790f1597b0c4a40ce621e6b1f75`
- p6.txt `d40112f13fde52af7d78fbece9a29ee987e23ad45eb745e6559b420d81de0f30`
- p7.txt `bfc4a39060910c00fe3064c0d9cf8c3e3daf462643101befc9a0f570e35ce0f4`
- p8.txt `2e70536e83db33c606d9a95ae103edf9dd11fde91180b1b861fbb3db39601267`

**Fresh twenty-four** — files in `prompts/` in this folder. SHA-256:
- l1.txt `08af07144b46b26273db656e5344c5c5ab9b5f43b612d5d81f972aacd062cf1c`
- l2.txt `ec5c60ba730bf8bab6ad34683f56fb956c018e3983d3d2e173d9bb1104f6ecec`
- l3.txt `be947b2e10df7c80363c01317248f9a5d6038aba63a3143c36c2ea91e6b1f44f`
- m1.txt `cca6d73db65400d6ce3d8d08e4f21111f24f7afdaef1d396223fffce332efc8e`
- m2.txt `e83d2b6d52ebb6c6f8ff29c3c580fb22d8f9cbaf3bf1218712e4d38148002af0`
- m3.txt `fd2cc9dba727f3eca500cc346d68ad886b2bc235966ef7c82361002197f81225`
- t1.txt `cbd3b5722a75eea3f1740fa83c2d2529ae04d92b23896b2d4a0b32a34c53c849`
- t2.txt `1c28e0b3fb39fa437f03db2d68a989c8b5b2a3ac5fb4480959f074ce657ab1d1`
- t3.txt `a1c00d6267738e4365e1d68633176a636d38dbf96a071a368ac20078fe739914`
- c1.txt `588d4fcaa0c80297124cafe9af9770d5f57f9dcbdd0a1caa1d81ed01854ce162`
- c2.txt `2016bd374fc39b7f34b6843d746121cf28cd40d0b952ae7cd965570510f1f4bc`
- c3.txt `fbfdaed74881e81ce89ea4b0ce292e438404a5f4b15c6b97d2b96f35a991c288`
- f1.txt `74013d7fab9be339a264e3848e027d9ec1e6a9c5e286e1868d112378ec6e5e6f`
- f2.txt `b59f372a10b0915c1132b01c17a4faf4474cb6d6ddef7fc53ae305f4b868f6c6`
- f3.txt `6b85c891f9dd90556c578d49035df6e8c490723a6687d4340fd602bb6b22f6d8`
- s1.txt `cc8a2a9339d85848898d90db0839e9bb07ffda8a7126e50377e9c75197ba4f82`
- s2.txt `cd482f8d8d383ea29e7f2ef974bf8f27d4046e3976d2e67792730055f0baa082`
- s3.txt `9a1d7f70b712aa31cbd5f7358811a23b7a1f81e9b859dfe3239776b2c1bccf99`
- e1.txt `12eec13a9d928d2fc3742f6d40b2266ea922e83246c3885c3c4dbeeda5e71236`
- e2.txt `9b257104a1d2d52058295d99f48e0a4591e6ea3e57c0b7d0b2f0fbae1f87dfc5`
- e3.txt `51532d7052b8a4cbb248f1098ebc8b0d6b132cf389acec0a5181d236d60a113b`
- w1.txt `e99b2430769583f7d8c8fbecc6c9b13f8f7f8baf689cf0df8259473f4ad52ccf`
- w2.txt `5e15058955673c6f9aeec57c5f74a4152ecc922a029b5ba88189de5b6c58425f`
- w3.txt `09481b34f4503cb188c9949eda7b3966d6c315b290294bed084d12c46f5cbfe2`

## Grading — delivered final answers only, against these keys

**What counts.** The delivered final answer is the text after the closing `</think>` of the response, as the demo displayed it. An answer **lands** if the think block closes (naturally or at the fence), answer text follows, and generation stops on its own (end-of-turn) before the 512-token cap, with the answer not trailing off mid-sentence. Grading applies to the delivered answer only — nothing inside the thinking counts, for or against.

**Counting mechanics (fixed now).** Words are whitespace-separated tokens. Sentences are counted by terminal punctuation (`.`, `!`, `?`) groups in the delivered answer, checked by reading. Bullets are lines beginning with a bullet marker (`-`, `•`, `*`) or a number; words per bullet exclude the marker. Lines are non-empty lines of the delivered answer. Code keys are verified by extracting the delivered function and running the listed cases; any import in the delivered code fails the no-imports clause.

**Original eight** (keys unchanged from the paired commitment):
- **P1:** Gold in A, silver in C, B empty — the only assignment under which exactly one statement is true.
- **P2:** $721.12 ($840 × 0.85 = $714.00; × 1.08 = $771.12; − $50 = $721.12).
- **P3:** 1969, Neil Armstrong. Naming Gagarin fails the trap.
- **P4:** Code is run: true median (mean of the two middle values for even counts), None on empty, no imports.
- **P5:** Exactly 4 bullet points, exactly 6 words in each, zero commas — counted mechanically.
- **P6:** Exactly 3 sentences, and both numbers from the passage appear (2,400 and 31,000). Declared flag, carried from the original record: the prompt says "both numbers" while the passage also contains years (1911, 1958, 1974); the key governs — the two counted quantities.
- **P7:** At most 3 sentences, and the explanation is the real one (sunlight scattered by air; blue scatters most).
- **P8:** Roughly 120 words (90–150 accepted), reads as a story opening, does not explain the letter.

**Logic (fresh):**
- **L1:** 1st Ada, 2nd Ben, 3rd Cleo, 4th Esme, 5th Dan. (Solved by hand at design time; the Ada–Ben pair at 4th–5th forces Esme 1st, which clue (4) forbids, so the order is unique.)
- **L2:** Pia knave, Quin knight, Rosa knave. (Pia knight leads to contradiction through Rosa's statement; Pia knave makes all three statements consistent, uniquely.)
- **L3:** Mara — dog — blue house; June — cat — red house; Theo — parrot — green house. (June red is forced because green would make her the parrot owner; the rest follows.)

**Money maths (fresh):**
- **M1:** $85.86 ($120 × 0.75 = $90.00; × 0.90 = $81.00; × 1.06 = $85.86).
- **M2:** $3.85 change (3 × $4.50 + 2 × $1.75 = $17.00; × 0.95 = $16.15; $20.00 − $16.15 = $3.85).
- **M3:** $93.00 ($63 × 2/3 = $42.00; + $4.50 = $46.50 per ticket; × 2 = $93.00).

**Trap questions (fresh):**
- **T1:** The ball costs $0.05 (5 cents). The trap answer, $0.10, fails: $0.10 + $1.10 = $1.20 ≠ $1.10.
- **T2:** 5 minutes (each machine makes one widget in 5 minutes; 100 machines work in parallel). The trap answer, 100 minutes, fails.
- **T3:** 1903, Orville Wright (at the controls for the first flight). Naming Wilbur fails the trap — he flew the longest fourth flight that day.

**Code (fresh)** — the delivered code is run, not just read:
- **C1:** `count_vowels("Hello")` = 2; `("AEIOU")` = 5; `("xyz")` = 0; `("")` = 0; `("Programming")` = 3. No imports.
- **C2:** `is_palindrome("Level")` = True; `("hello")` = False; `("Racecar")` = True; `("ab")` = False; `("aa")` = True. No imports.
- **C3:** `second_largest([1, 2, 3])` = 2; `([5, 5, 4])` = 4; `([7])` = None; `([3, 3])` = None; `([10, 9, 10, 9])` = 9. No imports.

**Tight-format writing (fresh)** — counted mechanically:
- **F1:** Exactly 3 bullet points, exactly 5 words in each, zero commas.
- **F2:** Exactly 2 sentences, exactly 8 words in each, zero occurrences of the whole word "the" (case-insensitive; "the" inside another word does not count).
- **F3:** Exactly 4 lines, exactly 4 words in each line, zero commas.

**Summary (fresh):**
- **S1:** Exactly 3 sentences, and both numbers from the passage appear (48 and 210). The passage contains no other numerals.
- **S2:** Exactly 3 sentences, and both numbers from the passage appear (1,150 and 9,600). Declared flag, same shape as P6: the prompt says "both numbers" while the passage also contains years (1923, 1967, 1989); the key governs — the two counted quantities.
- **S3:** Exactly 2 sentences, and the summary mentions the river, the bridge, and the market (case-insensitive).

**Plain explanation (fresh):**
- **E1:** At most 3 sentences, and the mechanism is the real one: light travels far faster than sound, so the flash is seen almost at once while the thunder takes longer over the same distance.
- **E2:** At most 3 sentences, and the mechanism is the real one: a perceptual illusion — the Moon's actual size in the sky stays essentially the same; the brain judges it against objects near the horizon. An answer claiming the Moon is physically larger, nearer, or magnified by the air fails.
- **E3:** At most 3 sentences, and the mechanism is the real one: water expands as it freezes, so ice is less dense than liquid water and floats.

**Story openings (fresh)** — the committed length rule from P8:
- **W1:** 90–150 words, reads as a story opening, does not explain the extra carriage.
- **W2:** 90–150 words, reads as a story opening, does not explain why the clock was built to tick slow.
- **W3:** 90–150 words, reads as a story opening, does not explain the sea taste.

## Reporting rule

results.md reports per prompt, per model: landed or not (and if not, why, from the trace), correct or not against the key above, with exact numbers. Where the window still closes first, the cause is named from the trace. Any prompt that proves ambiguous in grading is flagged by name. Where Llamatron is worse gets the same prominence as where it is better. Nothing in results.md may claim past what these 64 raw outputs show. Nothing is posted anywhere from this package.
