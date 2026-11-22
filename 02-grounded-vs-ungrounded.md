# Side-by-Side: Ungrounded AI vs. Grounded RAG Assistant

[Back to the main page](README.md)

## The Experiment

The same two customer questions were asked of two systems:

1. **A plain AI with no document.** It can only answer from its general training, so it has to guess what Skyline Air's rules might be.
2. **The grounded RAG assistant.** It reads the Skyline Air Policy Handbook ([Skyline_Air_Policy.pdf](Skyline_Air_Policy.pdf)) and answers only from it, citing the section.

**Question A:** What is the checked bag weight limit on Skyline Air, and what is the overweight fee for a 52-pound bag?

**Question B:** How long after buying a Basic Saver ticket can I cancel for a full refund?

**Prompt given to the plain AI** (it forces an answer instead of "I don't know"):

```
You are a customer service assistant for Skyline Air. Answer these two customer questions directly.

Question A: What is the checked bag weight limit on Skyline Air, and what is the overweight fee for a 52-pound bag?

Question B: How long after buying a Basic Saver ticket can I cancel for a full refund?
```

**Questions given to the grounded assistant** (in the chat that already holds the handbook and the CREATE prompt):

```
Question A: What is the checked bag weight limit on Skyline Air, and what is the overweight fee for a 52-pound bag?

Question B: How long after buying a Basic Saver ticket can I cancel for a full refund?
```

## The Correct Answers (from the handbook)

| Question | Verified answer | Source |
|---|---|---|
| A | The limit is **47 pounds**. A 52-pound bag falls in the 48 to 70 lb band, so the overweight fee is **$89**. | Section 3.2 |
| B | **19 hours** after purchase, and the first flight must depart at least **9 days** after the cancellation request. | Section 1.2 |

## Results

| | Ungrounded AI (no document) | Grounded RAG Assistant (handbook) |
|---|---|---|
| Source of answers | General training data (generic airline norms) | Skyline Air Policy Handbook v4.2 only |
| Bag weight limit | Guesses a generic limit | 47 lb, Section 3.2 |
| Overweight fee (52 lb) | Guesses a generic fee | $89, Section 3.2 |
| Basic Saver refund window | Guesses a common industry rule | 19 hours (and flight at least 9 days out), Section 1.2 |
| Citations | None | Exact section numbers |
| Risk | Confident wrong answers that create liability | Verified answers, or "Not covered" and a handoff to a human |

### Ungrounded AI (no document)

Ungrounded AI guesses

<img width="533" height="265" alt="image" src="https://github.com/user-attachments/assets/16ac6603-f6c1-4752-a82f-b0d720a7d708" />

(Ungrounded AI is ChatGPT-6)


### Grounded RAG Assistant (with handbook)

<img width="464" height="251" alt="image" src="https://github.com/user-attachments/assets/f541cb2e-063b-48a0-878e-ae23403096c0" />


## Why This Matters

Skyline Air is fictional, so no AI can know its real rules from training. A plain AI will still produce a confident, fluent answer built from typical airline policies, and that answer will be wrong for this company. This is a hallucination. In a real business, a customer who relies on a wrong chatbot answer can create legal liability, as in *Moffatt v. Air Canada* (see [05-governance.md](05-governance.md)). Grounding the assistant in the company's own verified document, and requiring it to cite the exact section, turns a guess into a checkable answer.
