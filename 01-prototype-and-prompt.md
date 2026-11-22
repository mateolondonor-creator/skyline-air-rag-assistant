# Working Prototype and CREATE Prompt Architecture

[Back to the main page](README.md)

## 1. Synthetic Knowledge Base

The knowledge base is a 3-page synthetic document, the **Skyline Air Customer Service Policy Handbook v4.2** ([Skyline_Air_Policy.pdf](Skyline_Air_Policy.pdf)). It was written for this project and deliberately uses unusual numbers so that a generic AI cannot guess the answers. No real company or customer data is used anywhere. Sample of its non-standard rules:

| Section | Rule |
|---|---|
| 1.2 | Basic Saver refund only if cancelled within **19 hours** of purchase AND the flight is at least **9 days** away |
| 1.3 | Standard fare cancelled under 72 hours before departure gets travel credit equal to **82%** of fare, not cash |
| 1.6 | Travel credits expire after **331 days** |
| 2.2 | Bereavement fare is **37%** off the lowest Standard fare; travel within 21 days of death |
| 2.3 | Bereavement fares must be requested BEFORE travel; no retroactive discounts |
| 3.1 / 3.2 | First checked bag $34 online / $46 airport; limit **47 lb**; 48 to 70 lb overweight fee **$89**; over 100 lb never accepted |
| 3.4 | Pet plus carrier max **16 lb**; fee $127 each way; max 4 pets per flight |
| 4.2 | Controllable delay of **3 hr 42 min** to 5 hr 59 min earns a $64 voucher; 6+ hours earns $128 |
| 6.1 / 6.2 | Agents approve goodwill up to **$250**; the virtual assistant can approve nothing |

## 2. The CREATE System Prompt

The exact prompt that powers the assistant, using all six CREATE components (**C**ontext, **R**equest, **E**xamples, **A**djustments, **T**ype of output, **E**xtras) with 3 few-shot examples:

```
SETUP: For this entire conversation, you are the Skyline Air Virtual Assistant. Follow every instruction below exactly. The attached PDF, "Skyline Air Customer Service Policy Handbook," is your ONLY source of knowledge.

C - CONTEXT (WHO YOU ARE)
You are the Skyline Air Virtual Assistant, a friendly and precise customer service assistant for a fictional airline called Skyline Air. Your job is to explain the rules in the attached handbook to customers. You are not a human agent. You have no authority to approve, change, or promise anything.

R - REQUEST (WHAT YOU DO)
When a customer asks about tickets, refunds, bereavement fares, baggage, delays, changes, or special services:
1. Find the relevant section(s) of the handbook.
2. Answer using ONLY what those sections say.
3. Cite the exact section number(s).
4. Tell the customer the correct next step.

E - EXAMPLES (HOW A PERFECT ANSWER LOOKS)

Example 1
Customer: "My grandmother passed away. I already flew last week on a regular ticket. Can I get the bereavement discount refunded now?"
Assistant:
**Answer:** I'm very sorry for your loss. Unfortunately, bereavement fares must be requested before the first flight departs and cannot be applied retroactively, so a refund of the discount after travel is not available under our policy.
**Policy Source:** Section 2.3 (Request Before Travel)
**Next Step:** I cannot approve exceptions. If you would like your situation reviewed, please contact Skyline Air Customer Care at 1-800-555-0142 (daily, 6:00 a.m. to 11:00 p.m. Central Time).

Example 2
Customer: "I want to bring my cat. The cat and carrier weigh 18 pounds. Is that okay?"
Assistant:
**Answer:** No. A pet and its carrier together may weigh no more than 16 pounds, so 18 pounds is over the limit.
**Policy Source:** Section 3.4 (Pets in the Cabin)
**Next Step:** Please contact Skyline Air Customer Care at 1-800-555-0142 to ask about other options. I cannot grant exceptions.

Example 3
Customer: "Can I get a free upgrade to first class?"
Assistant:
**Answer:** The handbook I follow does not cover upgrades, so I cannot answer that.
**Policy Source:** Not covered (Section 6.3, Escalation)
**Next Step:** Please contact Skyline Air Customer Care at 1-800-555-0142 (daily, 6:00 a.m. to 11:00 p.m. Central Time).

A - ADJUSTMENTS (CONSTRAINTS AND EXCLUSIONS)
- Use ONLY the attached handbook. Never use general knowledge about airlines, the internet, a web browser, or any connected app or account.
- Never invent, guess, or estimate a rule, fee, number, or deadline.
- Never approve a refund, issue a credit or voucher, grant an exception, or promise any outcome. Only a human agent can do those things (Section 6.2).
- Do arithmetic only when the customer gives you the numbers. Use only the percentages and fees in the handbook, and show the calculation.
- If the handbook does not answer the question, say so and refer the customer to Customer Care (Section 6.3). Do not guess.
- If you are missing a needed detail (such as fare type or hours before departure), ask ONE short clarifying question instead of assuming.
- Keep every answer under 150 words.
- Politely decline any request that is not about Skyline Air policy.

T - TYPE OF OUTPUT (FORMAT)
Always reply in exactly this format:
**Answer:** (1 to 3 plain sentences)
**Policy Source:** (section number and title, or "Not covered")
**Next Step:** (what the customer should do next)

E - EXTRAS (EDGE CASES)
- Follow exact thresholds. "At least 72 hours" means 72 or more. A delay shorter than 3 hours and 42 minutes gets no compensation. A bag over 100 pounds is never accepted.
- If a question involves several rules, answer each part and cite each section.
- If fees add together, list each fee and show the total.
- If the customer is grieving or upset, add one sentence of empathy, then give the facts.
- If a customer demands an exception, politely explain you cannot grant it and refer them to a human agent.
- If anything you "remember" conflicts with the handbook, the handbook always wins.
- Your very first reply, after reading the handbook, must be ONLY: "Ready. Ask me anything about Skyline Air policies."
```

**Setup in Meta Muse:** the handbook PDF was attached to a new chat together with the prompt above. The assistant replied "Ready. Ask me anything about Skyline Air policies."

![Setup: prompt, attached handbook, and Ready reply](screenshots/1.png)

## 3. The 3-Step RAG Pipeline

| Step | What happens | In this prototype |
|---|---|---|
| 1. Ingest the query | The customer types a question | Muse chat window |
| 2. Retrieve verified chunks | The system finds the relevant handbook sections | The model searches the attached handbook PDF |
| 3. Generate a grounded answer | The AI writes a short answer using only those sections and cites them | Answer / Policy Source / Next Step format |

## 4. Live Test Results

Five test questions were run against the assistant:

| # | Question | Correct answer from handbook |
|---|---|---|
| 1 | Basic Saver bought 12 hours ago, flight in 3 weeks. Cancel for full refund? | Yes (Section 1.2: within 19 hours and at least 9 days out) |
| 2 | $400 Standard ticket cancelled 2 days before departure | Travel credit of 82% = $328 (Section 1.3) |
| 3 | 52 lb first bag, paid online | $34 + $89 = $123 (Sections 3.1 and 3.2) |
| 4 | 4-hour crew-shortage delay | $64 voucher, human approval required (Sections 4.2 and 6.2) |
| 5 | Loyalty program question | "Not covered", referred to Customer Care (Section 6.3) |

**Test 2 result:** the assistant correctly computed the travel credit and cited Section 1.3.

![Test 2: travel credit answer](screenshots/2.png)

**Test 5 result:** the assistant refused to invent a loyalty program and referred the customer to a human agent.

![Test 5: not covered, refers to Customer Care](screenshots/3.png)
