# Governance and Security Audit

[Back to the main page](README.md)

## 1. Moffatt v. Air Canada (2024)

In *Moffatt v. Air Canada* (2024 BCCRT 149), Air Canada's website chatbot told a customer he could get a bereavement fare after buying his ticket. That wasn't the real policy. A British Columbia tribunal ruled the airline was responsible for its chatbot's mistake and ordered it to pay $650.88 plus interest and fees. Air Canada argued the chatbot was a separate legal entity, and the tribunal rejected that.

The takeaway is that a company owns what its AI says. This was a Canadian tribunal, so it isn't binding in the U.S., but it's the go-to example. My handbook's Section 2.3 (bereavement fares must be requested before travel) copies this exact situation.

## 2. Human-in-the-Loop (HITL) Gate

My assistant can only explain policy. It can't approve refunds, issue vouchers, or grant exceptions (Section 6.2). Anything involving money, an exception, or a question the handbook doesn't cover goes to a human agent (Section 6.3). That way a wrong answer gets caught by a person before the company pays or promises anything, which is the Moffatt risk. My red team tests in [03-red-teaming.md](03-red-teaming.md) show the gate holding up.

## 3. Closed-Tenant Licensing and the Samsung Incident

In 2023, Samsung employees reportedly pasted confidential source code into a public AI chatbot, and Samsung then restricted those tools. That's called Shadow AI, when employees use AI tools the company never approved. The fix is closed-tenant licensing: the company buys an enterprise license where its data stays in its own isolated environment and isn't used to train public models. My prototype used a free consumer tool, so I only used a fictional, synthetic handbook.

## 4. Non-Human Identities and Zero Trust

A Non-Human Identity (NHI) is a login for software, like a bot or API key. In production, my assistant would get its own NHI with minimal access: read-only on the handbook and permission to open a case ticket, with no access to payments or customer data. Zero Trust means every request is verified every time, even inside the company network. Micro-segmentation puts the assistant in its own isolated zone with no path to the payment system. So even if someone tricks it, like in my social-engineering test, it still can't issue a refund because it doesn't have access.

## Sources

- American Bar Association: https://www.americanbar.org/groups/business_law/resources/business-law-today/2024-february/bc-tribunal-confirms-companies-remain-liable-information-provided-ai-chatbot/
- McCarthy Tétrault: https://www.mccarthy.ca/en/insights/blogs/techlex/moffatt-v-air-canada-misrepresentation-ai-chatbot
- Pinsent Masons, OUT-LAW: https://www.pinsentmasons.com/en-gb/out-law/news/air-canada-chatbot-case-highlights-ai-liability-risks
