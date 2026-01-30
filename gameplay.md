# Gameplay Flow — The Carval Dossier (Full Process)

## Overview

This document describes the complete player flow from case briefing through every story card and question card, including timers, UI behavior, probability updates, attempts, penalties, and end conditions. Use this as the authoritative blueprint for implementing the single-file HTML/CSS/JS game.

---

# 1. Entry & Case Briefing

* Player opens game → sees a single “Case Briefing” screen.
* Elements shown:

  * Title: **The Carval Dossier**
  * Short summary: who Cassie is, where, and stakes (DPWH/flood-control corruption).
  * Button: **Accept Case** (starts the case).
* On Accept Case:

  * The game creates an internal suspect-probability vector (hidden to player): `[Ramon, Diego, Teresa, Liza, Miguel]` initialized **uniformly** (1/5 each), but these numbers are never shown.
  * The right sidebar appears visually but hides numeric probabilities: it shows five labeled suspect cards with empty/gray probability bars (no values). Bars visually fill only after the player answers questions; tooltips skeleton show only hints (not numbers).
  * A small HUD shows: “Case timer: OFF” (no global countdown yet). Individual question timers will appear per question as specified below.

---

# 2. Player-assigned Priors (First Interaction)

* Immediately after Accept Case, the player is prompted to **assign initial “gut priors”** based on the debrief (this is required).

  * UI: a card listing the five suspects and a 0–100 slider for each; sliders must sum to 100 (UI enforces).
  * Hint text: “Assign your current belief (%) based on alibis and initial facts — these are your working priors.”
  * If player prefers, a “Use default priors” button fills sliders evenly (20% each).
* When player clicks **Confirm Priors**:

  * These priors become the starting internal priors (normalized).
  * The sidebar bars remain visually minimal (no numbers) but reflect the player’s assigned priors as proportional fill (still no numbers).
  * The game records the player-chosen priors for later scoring.

---

# 3. Timeline & Card Navigation (Story → Question sequence)

* The case is divided into 15 phases. Each phase consists of:

  1. **Story Card** (narrative + images; left/right controls)
  2. **Question Card** (MCQ or solving input)
* Story Card behavior:

  * Each story card displays 1–3 illustrative images and text.
  * Controls: **Prev** (left) and **Next** (right). At first story card, Prev is disabled.
  * Next moves to the next Story Card; when the player reaches the end of the story card for that phase, the **Next** transitions to the **Question Card** for that phase.
  * Story cards are archived into the **Case History** (timeline) when the player moves forward; story card’s navigation buttons remain active while within the story card, but once the player advances to the question, the story card becomes read-only in history (can be revisited).
* Question Card behavior:

  * Displayed immediately after the phase’s final story card.
  * UI contains: the question prompt, input (MCQ or answer box), a **Submit** button, and a hint panel listing **components** needed to solve (no probability numbers). Example hint: “You will need P(Napkin|Miguel) and base rate for kitchen use — values provided in prompt.”
  * After the player submits and moves on, the question card’s interactive buttons (Submit, options) **disappear** and the card becomes a static entry in the **Case History**. The history allows reading the question & the player’s answer, but not resubmission.
  * The right sidebar updates visually (bars animate) to reflect internal posterior changes (still no numeric display). Hovering a suspect card shows a short hint of contributing evidence (e.g., “napkin found”, “partial print”) but not numerical values.

---

# 4. Attempt Rules & Penalties

* **MCQ (Questions 1–4)**:

  * Unlimited attempts.
  * On each attempt, the game provides immediate feedback: Correct → a short explanatory line and posterior update; Incorrect → hint encouraging retry.
  * No penalty for multiple tries.
* **Solving Questions (Questions 5–15)**:

  * Maximum **2 attempts** per question.
  * **On first correct attempt**:

    * Apply the defined update rule (see Section 6) to internal suspect probabilities; update sidebar bars (no numeric reveal).
  * **On first incorrect attempt**:

    * Provide a pedagogical hint (short explanation of what was misapplied).
    * Allow a second attempt.
  * **On second incorrect attempt (failure)**:

    * Apply a fixed **penalty distortion** to the internal probability vector to simulate investigative misdirection:

      * Penalty algorithm: multiply the true-killer candidate’s internal probability by `0.7` (reduce by 30%), then normalize across suspects; *and* add `+0.05` (5 percentage points of normalized posterior) equally to the two least-suspected suspects (spread evenly), then normalize.
    * Record a “confidence penalty” marker in the Case History for that question (visible textual note).
* Rationale: wrong solving answers should materially degrade the detective’s inference (simulating wrong assumptions harming the investigation).

---

# 5. Timers and Time Pressure

* There is **no single global countdown** in standard play to keep the player from feeling rushed.
* Certain investigative choices can **trigger a time penalty** (e.g., choosing to “Delay and investigate more” after a question). Implementation details:

  * If the player chooses to “Delay”, the game applies a small auto-penalty: shift `+0.03` probability mass to all non-primary suspects to represent opportunity slip. This is deterministic and visible only as sidebar animation.
* Optional: implement a “Challenge Mode” with a global case timer (e.g., 45 minutes) for hardcore play; if time runs out, player must make best-guess arrest with current posteriors.

---

# 6. Probability Update Rules (Exact, deterministic)

* All internal updates happen behind the scenes. Use these exact rules for code accuracy.

**A. MCQ update (instant, deterministic):**

* Correct MCQ: multiply the currently highest-probability suspect’s internal weight by `1.2` (boost 20%), then normalize.

**B. Solving (calculation) update on correct answer:**

* Most solving questions are labeled as one of two types:

  1. **Evidence Likelihood Update (Bayesian style)** — explicit Bayes updates:

     * When the question supplies `P(E | Suspect)` for a particular suspect and `P(E | not Suspect)`, compute the posterior for that suspect only using Bayes and then renormalize across all suspects.
     * Formula used:

       ```
       prior = P(S)
       likelihood = P(E | S)
       marginal = P(E) = likelihood * prior + P(E | notS) * (1 - prior)
       posterior = (likelihood * prior) / marginal
       ```
     * After posterior for that suspect is computed, replace that suspect’s prior with the computed posterior and renormalize the full vector across suspects.
  2. **Heuristic Multiplier Update (non-Bayes evidence):**

     * For evidence given as simple conditional or joint probability tasks where Bayes is not explicitly required, apply a multiplier to the implicated suspect(s): multiply suspect weight by `M` and normalize. Choose `M` as follows per question:

       * Strong evidence (e.g., partial fingerprint with high LR): `M = 3.0`
       * Moderate evidence: `M = 1.6`
       * Weak evidence: `M = 1.2`
* Always normalize after applying multipliers so probabilities sum to 1.

**C. Solving question incorrect (second failed attempt) penalty:**

* Apply penalty distortion described in Section 4.

---

# 7. Story Card → Question Card Mapping (sequence)

* There are 15 phases. Each phase’s Story Card(s) are presented first, then the Question Card. The exact flow:

1. **Phase 1 — Prologue** → Question 1 (MCQ: mutually exclusive)
2. **Phase 2 — The Invitation** → Question 2 (MCQ: mutually inclusive)
3. **Phase 3 — The Night Before (group scene)** → Question 3 (MCQ: independence conceptual)
4. **Phase 4 — The Safe** → Question 4 (MCQ: conditional concept)
5. **Phase 5 — Morning Discovery** → Question 5 (Solving: addition rule; initial priors shown as sliders earlier are used)
6. **Phase 6 — Initial Assessment** → Question 6 (Solving: addition rule inclusive, overlap)
7. **Phase 7 — The Empty Safe** → Question 7 (Solving: multiplication rule for motive & opportunity; independence assumed)
8. **Phase 8 — The Camera Gap** → Question 8 (Solving: conditional prob. P(Napkin & Miguel used kitchen) composite)
9. **Phase 9 — The Kitchen** → Question 9 (Solving: event joint probability, blood under bed inference)
10. **Phase 10 — Outside the House** → Question 10 (Solving: Bayes intro using napkin likelihoods)
11. **Phase 11 — Personal Items** → Question 11 (Solving: Bayes on burned pages → low LR update)
12. **Phase 12 — Burned Pages** → Question 12 (Solving: Bayes with fingerprint LR; strong evidence)
13. **Phase 13 — The Driver’s Words** → Question 13 (Solving: logical deduction about PIN structure)
14. **Phase 14 — The Second Phone** → Question 14 (Solving: passphrase input — free text)
15. **Phase 15 — Resolution** → Question 15 (Solving: final decision/arrest; no math input required but player must choose whom to arrest)

* After each question is answered (correct or after two attempts), game advances to the next story card.

---

# 8. Hints, Components & In-Card Help

* Each Question Card includes a **Hint Panel** that lists all numeric values and probabilities the player needs to solve the question. Example:

  * “Use: P(Napkin | Miguel) = 0.9, P(Napkin | not Miguel) = 0.3, prior(P(Miguel)) = (value derived from your internal prior — not shown).”
* The **prior value is not shown numerically** — instead the hint panel includes the **instruction**: “Use your current prior estimate for Miguel (your internal prior) — compute symbolically or rely on the game’s hint console to estimate.”

  * Implementation detail: to keep the game fair, hints will include all numeric external likelihoods; the prior is internal and not printed. Players are expected to calculate using the prior they set initially plus updates visualized by bar fill (qualitative).
* For accessibility, each question card provides a “Show worked example” after two failed attempts (on MCQ unlimited tries it remains available after first correct). The worked example describes the correct method but still penalizes if player had earlier wrong solves.

---

# 9. PIN and Phone Mechanics (final puzzles)

* **PIN discovery (Phase 13)**:

  * The story cards contain explicit narrative clues (napkin “2-7-6” twice, keypad scratch between digits 3 and 4, Cassie’s habit of grouping digits, audio beep grouping). Player must deduce the safe’s six-digit code.
  * Input: a 6-digit numeric field in the Question 13 card (free text solving).
  * Attempts: **3 attempts**.

    * Correct: the safe is considered solvable (game narrative advances; this unlocks additional evidence which will strongly increase Miguel posterior if correct), and the open-safe animation plays.
    * Incorrect attempts: after each wrong attempt, apply a small penalty shift `−0.02` to the true-killer internal probability (cumulative). After 3 failed attempts, the player is locked out of trying the PIN again until the final reveal — this simulates lost opportunity.
* **Second phone & passphrase (Phase 14)**:

  * The phone first requests a voice trigger, but the gameplay abstracts this: the question prompt explains the phone displays “Speak the trusted phrase.” The player must **type** the passphrase (free text).
  * The driver clue (Jane Austen/Dashwoods) is present in story cards and in Jose’s dialogue line; the passphrase expected: **DASHWOOD** (uppercase accepted).
  * Attempts: **Unlimited**, but each visible incorrect attempt triggers an explanatory hint and a minor penalty (`−0.03` to confidence in final arrest if the player abuses attempts).
  * Correct entry: phone auto-uploads dossier (narrative reveal) and provides a final evidence dump that mathematically increases the true killer’s posterior to near certainty (apply multiplier `M = 5.0` to true killer and renormalize).
  * If player refuses to enter passphrase (or fails), final arrest can still proceed based on probabilities.

---

# 10. Final Decision & End Conditions (Question 15)

* After Q15 (final question card), the player must **choose one suspect to arrest**.
* Success criteria:

  * If the chosen suspect’s internal posterior ≥ **0.75** → successful arrest; game shows full reveal and epilogue (including DASHWOOD effect if phone uploaded).
  * If chosen suspect’s posterior < 0.75 → **soft failure**: arrest fails, or wrongful arrest occurs; the game shows consequences (political fallout), but the player can view the reconstruction and how their incorrect calculations led to failure.
* If the player unlocked the phone and entered DASHWOOD earlier, the dossier gets uploaded → even if the arrest fails, the dossier publication triggers political consequences and an alternative epilogue.

---

# 11. Case History, Timeline & UI Rules

* **Case History (right/ bottom panel)** records every story card and question card in chronological order as the player advances. Cards in history are **read-only** once the player has moved on (question cards show player's answer and whether it was correct or penalized).
* **Interactive buttons on a question card disappear** after moving forward; story cards remain navigable (prev/next) only until their original question is answered. After that, story card navigation still allows reading but not re-activating the question.
* **Sidebar suspect cards** animate to show relative weights but never display numeric probability values. Hover reveals short textual hints (evidence items that contributed), but not numbers.

---

# 12. Scoring and Feedback

* The game calculates a final score composed of:

  * Correctness of answers (weighted)
  * Number of failed solving attempts (penalty)
  * Whether the player unlocked the phone (bonus)
  * Time used (if in Challenge Mode)
* After the final reveal, the game shows:

  * Narrative epilogue (true sequence, who did what),
  * A reconstruction that shows how each answered question changed internal posteriors (numerical debug view can be toggled in a “Detective Report” for learning purposes).

---

# 13. Data & Debug Mode (Developer option)

* Optional toggle (developer/learning mode) reveals internal numeric priors/posteriors after each question for study mode only. This is off by default and not part of the normal player experience.

---

# 14. UI Micro-interactions & Accessibility Notes

* All text inputs should tolerate whitespace and case-insensitive matching (e.g., DASHWOOD accepted as dashwood, Dashwood).
* Hints and help panels are keyboard-accessible.
* Images described in story cards are required; alt-text is provided for screen readers.
* All gameplay events (posterior updates, penalties) have short descriptive messages for clarity (e.g., “Evidence strengthens Miguel’s likelihood — sidebar updates.”).

---

# Appendix: Concrete numeric rules summary (for code)

* Initial priors: use player's chosen sliders (sum to 1.0).
* MCQ correct: multiply current top suspect weight by `1.2`, normalize.
* Solving correct (Bayes): apply exact Bayes formula to affected suspect then normalize full vector.
* Solving correct (heuristic): multiply implicated suspect(s) by `M` (3.0 strong, 1.6 moderate, 1.2 weak), normalize.
* Solving failed on 2nd attempt: true-killer weight ×= 0.7; add `+0.05` mass equally to two lowest suspects; normalize.
* PIN wrong attempt: after each wrong PIN attempt subtract `0.02` from true-killer posterior (via scaling) and renormalize.
* Phone wrong attempt: each wrong try subtract `0.03` from final confidence multiplier (accumulate).
* Final arrest success threshold: chosen suspect posterior ≥ `0.75`.

---
