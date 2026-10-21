# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy is at least 0.70 on the held-out set, across all three seeds.

**Why this target:** A random guesser across three balanced labels gets about 0.33. A model that reaches 0.70 is roughly twice as useful as chance, which feels like the floor for something worth using. Because my labels are roughly equal in count (~70 each), accuracy isn't distorted by one dominant class, so 0.70 is a fair read.

---

## 2. Macro F1 is at least 0.65 on the held-out set, across all three seeds.

**Why this target:** Macro F1 averages across all three labels equally, so a model can't hide a weak label behind two strong ones. 0.65 is slightly below the accuracy target because F1 penalises precision and recall mismatches that accuracy ignores. If the model learns to recognise one label by aggressively over-predicting it, macro F1 will catch that where accuracy might not.

---

## 3. Every individual label has an F1 of at least 0.55 on the held-out set, across all three seeds.

**Why this target:** With ~70 examples per label and a 15% test split, each label lands in the test set about 10–11 times. F1 on that few posts swings a lot between seeds, so I'm setting 0.55 rather than the 0.65 macro target. The hardest boundary in this taxonomy is `analysis` vs `hot_take` — a post can cite a number but still be a `hot_take` if the fact isn't doing the argumentative work — so I expect at least one label to underperform and 0.55 gives it room to still count as working.

---

## 4. No single label exceeds 40% of the total dataset.

**Why this target:** With three labels a perfectly even split would be 33% each. I'm allowing up to 40% because collecting in the wild means some post types are more common, but beyond 40% the model starts learning to guess the majority label and the minority labels' F1 scores become unreliable. My current counts are close to 70 each so this should hold, but I'm stating it explicitly as a check before training.

---

## 5. Agreement with the staff set is at least 70% on the 30 shared posts.

**Why this target:** The staff taxonomy uses the same three labels as mine, which makes it a genuine consistency check rather than just a label-mapping exercise. 70% is the rate the brief describes as normal and expected for posts chosen because they're genuinely ambiguous. Hitting at least 70% means my decision rules are tight enough to agree with a second annotator most of the time; falling below it would suggest I applied my own rules inconsistently across my 200 posts.



---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
