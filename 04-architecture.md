# System Architecture: The Restaurant Analogy

[Back to the main page](README.md)

To explain how my assistant works to non-technical people, I mapped it to a restaurant: the customer sits in the dining room, a waiter carries orders, the kitchen cooks, and the fridge stores the ingredients.

```mermaid
flowchart TD
    A[Dining Room - Muse chat window] --> B[Waiter - Secure web request]
    B --> C[Kitchen - Muse AI plus CREATE prompt]
    C <--> D[Fridge - Skyline policy handbook PDF]
    C --> E{HITL gate - approval needed}
    E -->|No - policy explanation only| F[Answer returned to customer]
    E -->|Yes or not covered| G[Human agent - Customer Care decides]
```


## The Parts

**Dining Room (front-end).** This is the chat window in Meta Muse where the customer types a question and reads the answer. They never see the rules or the AI behind it.

**Waiter (API layer).** The waiter carries the question to the AI and brings the answer back. In my prototype this is just Muse's secure web connection. In a real company it would also use
