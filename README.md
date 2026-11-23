# AI Workflow Architect: Skyline Air Policy Assistant (Grounded RAG, Meta Muse)

**BUAN 3301: AI in Business | Final Capstone Sprint**


> **Privacy note:** This project uses only a fictional airline and 100% synthetic data. No real customer, employee, or company data was entered into any tool.

## Project Summary

| | |
|---|---|
| **Domain** | Customer Service |
| **Use case** | A virtual assistant for the fictional airline Skyline Air that answers refund, baggage, delay, and bereavement-fare questions using only the official policy handbook, cites the exact section, and hands anything uncertain to a human agent. |
| **Type of AI** | Grounded RAG (Retrieval-Augmented Generation) assistant |
| **Value type** | Cost Reduction (primary), Risk Mitigation (secondary) |
| **Tool** | Meta Muse (free tier) |
| **Quick Win?** | Yes. High Business Impact: routine policy questions are a large share of support contacts. High Technical Feasibility: one document and a no-code tool. Low risk: the assistant explains rules but never approves anything. |

**What is RAG?** Instead of answering from memory (where AI can make things up), the assistant first retrieves the relevant paragraphs from a trusted document, then writes its answer using only those paragraphs and cites the section. It is an open-book exam where the AI must point to the page.

## Headline Results

- **Projected first-year ROI:** 330.4%
- **Payback period:** 2.8 months
- **Model routing savings:** 72% lower token cost than running everything on a frontier model
- **Red team:** 3 adversarial attacks tested and blocked, with the system prompt hardened after each

## Contents

| Part | File |
|---|---|
| 1. Working prototype and CREATE prompt | [01-prototype-and-prompt.md](01-prototype-and-prompt.md) |
| 1. Grounded vs. ungrounded comparison | [02-grounded-vs-ungrounded.md](02-grounded-vs-ungrounded.md) |
| 2. Red team stress-testing | [03-red-teaming.md](03-red-teaming.md) |
| 2. Restaurant Analogy architecture | [04-architecture.md](04-architecture.md) |
| 3. Enterprise governance and security audit | [05-governance.md](05-governance.md) |
| 4. Tokenomics and CFO business case | [06-tokenomics-and-roi.md](06-tokenomics-and-roi.md) |
| Synthetic knowledge base (PDF) | [Skyline_Air_Policy.pdf](Skyline_Air_Policy.pdf) |

## AI Use Disclosure

This project was built with AI assistance, consistent with the course's use of generative AI tools. Claude (Anthropic) helped me draft the synthetic policy document, system prompt, governance text, and financial model, and Meta Muse was the prototype platform. I ran and screenshotted every test and Red Team attack myself and recorded the video pitch myself.
