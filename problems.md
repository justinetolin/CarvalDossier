Great — below is a **complete, playable set of 15 questions**, written exactly as the detective/player experiences them.
Each question includes:

* Narrative context (what the detective just learned)
* Question format (MCQ or solving)
* Topic
* Given probabilities / data (so it’s answerable)
* What the answer *does narratively* (probability update intent)
* Where the PIN and Jane Austen clues are embedded (without revealing answers outright)

I will **not** reveal the PIN or the killer here — the questions are designed so the player figures them out.

––––––––––––––––––––
QUESTIONS 1–4 (MULTIPLE CHOICE, UNLIMITED ATTEMPTS)
These are intuition-building and grounding questions.

QUESTION 1
Topic: Mutually Exclusive Events
Format: Multiple Choice

Narrative context:
The detective establishes that Cassie was killed inside her house and that all six individuals slept in the house that night.

Question:
Which of the following pairs of events are mutually exclusive?

A. “The killer is a guest” and “The killer is a government official”
B. “The killer entered through the front door” and “The killer entered through the back door”
C. “The killer is Miguel Reyes” and “The killer is Senator Liza Mendoza”
D. “The killer had motive” and “The killer had opportunity”

Correct logic:
Only one person can be the killer.

Narrative effect:
Teaches exclusivity of suspects — only one suspect can be guilty.

––––––––––––––––––––

QUESTION 2
Topic: Mutually Inclusive Events
Format: Multiple Choice

Narrative context:
The detective reviews motives. Several suspects benefited from flood-control corruption.

Question:
Which pair of events can occur at the same time?

A. “The suspect works in DPWH” and “The suspect is implicated in flood-control anomalies”
B. “The suspect was asleep” and “The suspect was seen on camera at 00:25”
C. “The suspect destroyed evidence” and “The suspect is innocent of murder”
D. “The suspect had no motive” and “The suspect stole the dossier”

Correct logic:
Destroying evidence does not necessarily mean murder.

Narrative effect:
Prepares player for Senator Liza’s burned pages without jumping to guilt.

––––––––––––––––––––

QUESTION 3
Topic: Independent Events
Format: Multiple Choice

Narrative context:
The camera glitch occurred between 00:20–00:23.

Question:
Which statement best describes the camera glitch and the killer’s motive?

A. The glitch caused the motive
B. The motive caused the glitch
C. The glitch and motive are independent events
D. The glitch proves intent to kill

Correct logic:
System failure and human motive are independent.

Narrative effect:
Separates opportunity from intent.

––––––––––––––––––––

QUESTION 4
Topic: Conditional Probability (Conceptual)
Format: Multiple Choice

Narrative context:
The safe was opened without force.

Question:
Which statement is most reasonable?

A. If the safe was opened, then the killer must be Cassie
B. If the killer knew the PIN, then the safe could be opened
C. If the safe was opened, then everyone knew the PIN
D. If the PIN was known, then murder was inevitable

Correct logic:
Knowing the PIN enables access but does not guarantee murder.

Narrative effect:
Introduces conditional reasoning without math.

––––––––––––––––––––
QUESTIONS 5–15 (SOLVING QUESTIONS)
Limited attempts. Wrong answers distort suspect probabilities.

––––––––––––––––––––

QUESTION 5
Topic: Addition Rule (Mutually Exclusive)

Narrative context:
Initial suspect pool:

* Ramon (DPWH Usec)
* Diego (Speaker)
* Teresa (Congresswoman)
* Liza (Senator)
* Miguel (Legal Counsel)

Given:
At the start, all suspects are equally likely.

Question:
What is the probability that the killer is either Ramon OR Diego?

Solution:
P = 1/5 + 1/5 = 2/5

Narrative effect:
Initial probability board appears.

––––––––––––––––––––

QUESTION 6
Topic: Addition Rule (Mutually Inclusive)

Narrative context:
Three suspects are implicated in DPWH flood-control anomalies: Ramon, Diego, Teresa.
Two suspects were present during the camera glitch window: Liza, Teresa.

Question:
What is the probability that a suspect is implicated OR present during the glitch?

Solution:
P(A ∪ B) = P(A) + P(B) − P(A ∩ B)
= 3/5 + 2/5 − 1/5 = 4/5

Narrative effect:
Suspicion broadens — many could be involved.

––––––––––––––––––––

QUESTION 7
Topic: Independent Events with AND / OR Logic (Addition & Multiplication Rules)

Narrative context:
Investigators conclude that the killer must have either personally known the six-digit safe code OR gained access while Cassie was distracted—but to successfully remove the dossier unnoticed, the killer still needed uninterrupted access to the study. Investigators focus on Miguel Reyes, whose legal work often brought him into Cassie’s study that night.

Given:
P(Miguel knows the safe code) = 0.42  
P(Miguel gains access to the study without knowing the code) = 0.23  
P(Miguel has uninterrupted access to the study) = 0.59  

Assumptions:
- Knowing the code and gaining access without knowing the code are mutually exclusive events.
- Uninterrupted access to the study is independent of how Miguel gained entry.

Question:
What is the probability that Miguel could have taken the dossier, (meaning that he either knew the safe code OR gained access without knowing it) AND had uninterrupted access to the study?

Solution:
First, compute the OR component (mutually exclusive):
P(Knowing code OR gaining access without knowing code)  
= 0.42 + 0.23  
= 0.65  

Then, combine with uninterrupted access (independent event):
P(Took dossier)  
= 0.65 × 0.59  
= 0.3835  (full 4 decimals is required)

Narrative effect:
Miguel’s likelihood increases substantially, as he plausibly had both the means and the opportunity to remove the dossier without detection.

––––––––––––––––––––

QUESTION 8
Topic: Conditional Probability

Narrative context:
One suspect was known to be awake and moving through the house during that exact window.

Given:
P(Camera blackout) = 0.3  
P(Suspect moving during blackout | Camera blackout) = 0.7  

Question:
What is the probability that the suspect was moving through the house during the blackout window?

Solution:
P(Suspect moving during blackout)  
= P(Camera blackout) × P(Suspect moving during blackout | Camera blackout)  
= 0.3 × 0.7 = 0.21  

Narrative effect:
Suspects with verified movement during the blackout become more likely to have acted unnoticed, increasing their probability.

––––––––––––––––––––

QUESTION 9
Topic: Conditional Probability

Narrative context:
A napkin with digits “2-7-6” is found in the kitchen trash.

Given:
P(Miguel used the kitchen) = 0.58
P(Napkin found | Miguel used kitchen) = 0.86

Question:
What is P(Napkin found AND Miguel used kitchen)?

Solution:
0.58 × 0.86 = 0.5

Narrative effect:
Miguel’s probability increases.

––––––––––––––––––––

QUESTION 10 (Investigate Miguel OPTION) (should increase Miguel's probability)
Topic: Bayes Theorem (Intro)

Narrative context:
The detective reassesses Miguel.

Given:
P(Miguel is killer) = 0.2 (based on the sidebar/current probability)
P(Napkin digits | Miguel is killer) = 0.96
P(Napkin digits | Miguel innocent) = 0.34

Question:
What is P(Miguel is killer | Napkin digits)?

Solution:
Numerator = 0.96 × 0.2 = 0.192
Denominator = 0.192 + (0.34 × 0.8) = 0.42
Posterior = 0.18 / 0.42 ≈ 0.43

Narrative effect:
Miguel becomes leading suspect.

––––––––––––––––––––

QUESTION 10 (Inspect the Footprints OPTION) (should lower Miguel's probability but the leads to discovery of the murder weapon wrapped in a cloth)
Topic: Bayes’ Theorem

Narrative context:
Muddy footprints were discovered leading from the back door toward the garden and then back inside. Investigators theorize that the killer wrapped the knife in a cloth before briefly stepping outside to discard or clean it. One suspect had mud on their shoes the next morning.

Given:
P(Suspect is the killer) = 0.20 
P(Muddy shoes | Suspect is the killer) = 0.93  
P(Muddy shoes | Suspect is not the killer) = 0.27  

Question:
Given that muddy shoes were observed, what is the probability that this suspect is the killer?

Solution:
Bayes: P(A|B) = P(B|A)P(A) / P(B)
P(Killer | Muddy shoes) = P(Muddy shoes | Killer) × P(Killer) / P(Muddy shoes) 
= (0.93 × 0.2) / [(0.93 × 0.2) + (0.27 × 0.8)]  
= 0.18 / (0.18 + 0.24)  
= 0.18 / 0.42  
≈ 0.43  

Narrative effect:
The suspect with muddy shoes becomes significantly more suspicious, but the evidence is not conclusive—other explanations remain possible.

--------------------

QUESTION 11
Topic: Conditional Probability

Narrative context:
Hemoglobin trace is found under Miguel’s bed.

Given:
P(Killer | Blood under bed) = 0.72
P(Blood under bed) = 0.24

Question:
What is P(Killer AND blood under bed)?

Solution:
0.72 × 0.24 = 0.1728

Narrative effect:
Strong but not definitive evidence.

--------------------

QUESTION 12 - latest
Topic: Bayes Theorem

Narrative context:
Burned dossier fragments are found in Senator Liza’s trash.

Given:
P(Burned pages | Liza killer) = 0.61
P(Burned pages | Liza innocent) = 0.55

Question:
Update P(Liza killer | burned pages).

Solution:
0.61 × 0.2 / [(0.61 × 0.2) + (0.55 × 0.8)] = 0.12 / 0.52 ≈ 0.23

Narrative effect:
Burning evidence does not strongly imply murder.

––––––––––––––––––––

QUESTION 13
Topic: Bayes’ Theorem

Narrative context:
A partial fingerprint is discovered on the rim of Cassie Carval’s safe. Forensic analysis confirms that the print is consistent with Miguel Reyes. Investigators note that Miguel had legitimate access to the study earlier that evening, so the presence of a fingerprint alone is not definitive—but its location on the safe makes it highly relevant.

Given:
P(Print | Miguel is the killer) = 0.85  
P(Print | Miguel is innocent) = 0.14  

Assume:
Current prior belief that Miguel is the killer:
P(Miguel is the killer) = 0.43  
Therefore:
P(Miguel is innocent) = 1 − 0.43 = 0.57  

Question:
Given the fingerprint evidence, what is the updated probability that Miguel is the killer?

Solution:
Apply Bayes’ Theorem:

P(Miguel | Print)  
= [P(Print | Miguel) × P(Miguel)]  
  / [P(Print | Miguel) × P(Miguel) + P(Print | Miguel innocent) × P(Miguel innocent)]

= (0.85 × 0.43) / [(0.85 × 0.43) + (0.14 × 0.57)]  
= 0.3655 / (0.3655 + 0.0798)  
= 0.3655 / 0.4453  
≈ 0.82  

Narrative effect:
The fingerprint dramatically shifts suspicion toward Miguel. While not absolute proof, the probability now indicates near certainty that he accessed the safe during the critical window.


––––––––––––––––––––

QUESTION 14
Topic: Final Bayesian Convergence

Narrative context:
All evidence is combined.

Given:
Final probabilities (consider actual probabilities of suspects from the suspect probabilities sidebar)

Question:
Who should be arrested?

Narrative effect:
Case closes on who the killer is.

––––––––––––––––––––

QUESTION 15
Topic: Conditional Reasoning (Voice Authentication)

Narrative context:
Cassie’s second phone activates and asks:
“Speak the trusted phrase.”

Driver’s statement earlier:
“She used to say… some people choose sense, others choose feeling. But the Dashwoods had both.”

Question:
What is the most likely passphrase?

Player types answer.

Narrative effect:
Correct entry reveals files.