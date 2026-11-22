# Governance and Security Audit

[Back to the main page](README.md)

## 1. Corporate Liability: Moffatt v. Air Canada (2024)

In *Moffatt v. Air Canada* (2024 BCCRT 149), decided February 14, 2024, a British Columbia tribunal ruled that Air Canada had to take responsibility for wrong information its own website chatbot gave a customer. The chatbot told a customer who was booking flights after his grandmother died that he could apply for a bereavement fare after buying his ticket, which was not the airline's real rule. The tribunal found Air Canada negligent for not keeping its chatbot accurate, which is called negligent misrepresentation, and ordered it to pay $650.88 in damages plus $36.14 in interest and $125 in fees. Air Canada argued the chatbot was its own separate legal entity, and the tribunal rejected that. The tribunal also said the customer shouldn't have been expected to double-check the chatbot against other pages on the website.

The takeaway is that a company owns everything its AI says. The ruling is from a Canadian tribunal, so it isn't binding in the U.S., but it is widely used as the warning example for chatbot liability. My Skyline Air handbook copies this exact situation in Section 2.3 (bereavement fares must be requested before travel), and my CREATE prompt has an example that gives the correct answer.

## 2. Runtime Control: The Human-in-the-Loop (HITL) Gate

My assistant can only **explain** policy. It can never approve a refund, issue a voucher, grant an exception, or promise anything (Handbook Section 6.2). Anything that involves money, an exception, or something the handbook doesn't cover goes to a human agent at Customer Care (Section 6.3), and goodwill over $250 needs a supervisor (Section 6.1).

This lowers the Moffatt risk in two ways:

- Every answer comes from a verified document and cites a section, so the assistant can't invent a policy.
- If it's ever wrong or unsure, a trained person reviews the decision before the company pays or promises anything.

My three Red Team tests (see [03-red-teaming.md](03-red-teaming.md)) show that the gate holds against prompt injection, fake authority, and out-of-range numbers.

## 3. Data Protection: Closed-Tenant Licensing and the Samsung Incident

In 2023, Samsung employees reportedly pasted confidential source code and internal meeting notes into a public AI chatbot to get help with work, and Samsung then restricted employee use of these tools. This is a classic example of **Shadow AI**, which means employees using AI tools the company never approved.

In production, IT would prevent this with **closed-tenant licensing**. The company buys an enterprise AI license where its data stays in its own isolated environment, isn't used to train public models, and is protected with single sign-on, admin controls, and audit logs. When employees have an approved tool that works well, they have less reason to go around it.

My prototype ran on a free consumer tool, so I used only a fictional, synthetic handbook. No real customer or company data was entered, which follows the course privacy rule.

## 4. Access Control: Non-Human Identities and Zero Trust Micro-Segmentation

A **Non-Human Identity (NHI)** is a login for software instead of a person, like a service account, API key, or bot. In production, my assistant would get its own NHI with the least access it needs: read-only access to the policy handbook and permission to create a case ticket for a human, and nothing else. It would have no access to payment systems, refund tools, or customer personal data. Its credentials would be short-lived, rotated automatically, and logged.

This works with **Zero Trust**, which means never trust and always verify. Every request from the assistant is checked every time, even from inside the company network. **Micro-segmentation** splits the network into small isolated zones, so the assistant sits in its own zone with no route to the payment system. If it were ever tricked, like in my social-engineering test, it still couldn't issue a refund, because it doesn't have the access, not just because it was told not to.

## Sources (Moffatt v. Air Canada)

- American Bar Association, Business Law Today (Feb. 2024): https://www.americanbar.org/groups/business_law/resources/business-law-today/2024-february/bc-tribunal-confirms-companies-remain-liable-information-provided-ai-chatbot/
- McCarthy Tétrault (Feb. 2024): https://www.mccarthy.ca/en/insights/blogs/techlex/moffatt-v-air-canada-misrepresentation-ai-chatbot
- Pinsent Masons, OUT-LAW News: https://www.pinsentmasons.com/en-gb/out-law/news/air-canada-chatbot-case-highlights-ai-liability-risks
