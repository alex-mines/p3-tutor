# Open: the model "runs" tools (M4), and an agent needs a stopping condition (M6)

On the spaced retry of quiz 0001 (s2), Riley traced the loop correctly and counted four model calls, and explained that tool results go back as the next input. Two misconceptions remain: that the model runs the tools itself (M4, in the reasoning on q4), and that an agent left to run longer will eventually succeed (M6, q3).
Riley also relies on a tool's name alone to explain how the model picks it (q6), not the full description.

**Evidence:** quiz 0001, submission s2, items q3, q4, q6.

**Implications:** Before building on tool design or budget, check M4 and M6 again, for example by asking Riley to write a tool description and a step limit for the makerspace assistant.
