# Tokenomics and CFO Business Case

[Back to the main page](README.md)

AI is billed in **tokens** (about 3/4 of a word each), like a utility bill. Two ways I keep that bill low: send easy questions to cheap models, and limit how much text the AI reads and writes.

## 1. Model Routing

| Tier | Example models | Handles | Share of volume |
|---|---|---|---|
| Cheap (distilled) | Claude Haiku or DeepSeek | Sorting questions, routine lookups (bag fees, refund windows), formatting answers | 90% |
| Frontier | Claude Opus or GPT-5 | Questions that combine several rules, upset or grieving customers, suspicious messages, weekly quality audits | 10% |

Prices are my assumptions (real prices change, so a company would check the vendors' pricing pages). Cheap tier: $1 per million input tokens, $5 per million output. Frontier tier: $5 per million input, $25 per million output.

**Cost per conversation** (4,000 input tokens, 500 output tokens):
- Cheap: (4,000 × $1 ÷ 1,000,000) + (500 × $5 ÷ 1,000,000) = $0.004 + $0.0025 = **$0.0065**
- Frontier: (4,000 × $5 ÷ 1,000,000) + (500 × $25 ÷ 1,000,000) = $0.020 + $0.0125 = **$0.0325**
- Blended: (0.90 × $0.0065) + (0.10 × $0.0325) = $0.00585 + $0.00325 = **$0.0091**

For 120,000 conversations a year: 120,000 × $0.0091 = **$1,092**. Using only the frontier model would cost 120,000 × $0.0325 = $3,900. Routing saves $3,900 − $1,092 = **$2,808, or 72%**.

## 2. Context Window Limits

| Control | Limit | Why |
|---|---|---|
| Policy sections retrieved per question | Top 3 | The AI only reads what it needs |
| Max input per request | 4,000 tokens | Keeps cost per chat predictable |
| Max output per answer | 500 tokens (prompt also caps answers at 150 words) | No long, expensive replies |
| Chat history kept | Last 6 messages | Old messages get dropped so the bill doesn't grow |

My handbook is short enough to read whole, but a real airline manual could be hundreds of pages. Pulling only the relevant sections is what lets this scale.

## 3. One-Year Financial Model

Pretend a mid-sized airline deploys this.

**Assumptions**
- 200,000 customer contacts a year; 60% are routine policy questions, so **120,000** reach the assistant
- The assistant fully resolves 60% of those, so **72,000** resolved
- A human agent spends 6 minutes on one of these, at $30/hour fully loaded, so **$3.00 per contact**
- Only 50% of the saved time becomes real cash savings (the rest is agents moving to harder work)

**Total Investment**

| Item | Math | Cost |
|---|---|---|
| Setup labor (build, test, red team) | 160 hrs × $60 | $9,600 |
| Policy and legal review | 40 hrs × $90 | $3,600 |
| API tokens | from section 1 | $1,092 |
| Cloud hosting and vector database | $300 × 12 | $3,600 |
| Human review of 2% of chats | 2,400 chats × 3 min = 120 hrs × $30 | $3,600 |
| Maintenance | 5 hrs/month × 12 × $60 | $3,600 |
| **Total** | | **$25,092** |

**Total Value**
- Labor saved: 72,000 × $3.00 = $216,000
- At 50% realized: $216,000 × 0.50 = **$108,000**

**ROI** = (Total Value − Total Investment) ÷ Total Investment × 100
= ($108,000 − $25,092) ÷ $25,092 × 100
= $82,908 ÷ $25,092 × 100 = **330.4%**

**Payback period:** monthly value = $108,000 ÷ 12 = $9,000, so $25,092 ÷ $9,000 = **2.8 months**.

I left out revenue from better service and the cost of a *Moffatt*-style lawsuit, so the case stays conservative.

## 4. Success KPIs

| Type | KPI | Target |
|---|---|---|
| Hard | Containment rate: share of policy chats resolved with no human | 60% or higher |
| Hard | Answer accuracy: share of audited answers with the right policy and section | 98% or higher |
| Soft | Agent burnout: agents spend less time on repetitive questions | Checked with quarterly agent surveys |
| Soft | Customer trust: customers feel they got a clear, honest answer, even when it's "talk to a person" | Checked with feedback comments |
