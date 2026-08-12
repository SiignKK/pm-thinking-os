# Outcome review protocol

Review compares the original judgment with later observation. It is not a
performance evaluation.

Before asking anything, show the original Decision, Assumptions, Prediction,
and Falsification Signal exactly as stored.

Then ask one question per turn:

1. What actually happened, and what source supports it?
2. How did it differ from the prediction?
3. Which assumption was supported, rejected, or remains unknown?
4. Which external factor materially affected the result?
5. Was the original process reasonable using information available then?
6. What should be retained or changed next time?

Keep **Process Quality** separate from **Outcome Quality**. A good process can
produce a bad outcome; a weak process can get lucky.

The confirmed review requires:

- Actual Outcome: observed result and source.
- Prediction Difference: expected versus actual.
- Assumption Updates: one or more supported, rejected, or unknown judgments.
- External Factors: one or more material confounders, or `none observed`.
- Process Quality: assessment using information available at decision time.
- Outcome Quality: assessment of the observed business result.
- Next Time: a specific future reminder.

Render the review for confirmation before saving. Follow the append-only shape
in [storage-protocol.md](storage-protocol.md), use a new UTC timestamp, and
never edit the original Decision, Prediction, or previous reviews.
