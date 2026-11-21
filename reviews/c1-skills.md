# Reviews: c1-skills

Your quick rounds while you build `c1-skills`, and then its major review, once you have snapshotted it and made its full run.
In this step no round adds a subagent, because subagents arrive in `c2-review` and `c3-split`: a round changes or adds a skill, or makes another change the skills need, such as an allow rule (Kind `other`).
Your first round can start from the tutor as it ships, so the baseline is the output without your first change.
The major review covers all four jobs, and its previous step is `c0-baseline`, so compare with your baseline runs.
What it finds becomes the checklists of your reviewer subagents in `c2-review`, so be specific.
`reviews/README.md` says how to write each part.
You write your thoughts, ratings and feedback; Claude Code writes only the What changed part of each round.
Keep every heading as it is, because `p3 check` reads them, and add a round by copying the last one with its marker lines.

## Quick rounds

### Round 1

**Run:** bench/runs/_live/20261120-210520-agent-architecture-m30-r1, a quick run

**My thoughts:**

In the baseline, p01 (201813) and p06 (201133) failed on reply length. A good, measurable first change is a self-check step in the tutor: after drafting a reply, count the words and revise it to meet the limit before sending. This should fix the length failures. It won't fix p01's main failure, where the hints narrow the options, so I'll address that in a later round.

**What changed:**

- Kind: modified skill
- Files: tutor/.claude/skills/teach/references/TUTORING.md
- Why: Implemented the self-check that counts the words of a drafted reply and cuts it to the limit; the chat-reply rule now requires a count and a cut to 200 words or fewer before sending, which covers both the p01 and p06 limits.

### Round 2

**Run:** bench/runs/_live/20261120-222318-agent-architecture-m30-r1, an --only run of p01, p02 and p06

**My thoughts:**

The length change helped partly. p06 now passes, with a reply of about 225 words, where it failed on length in the baseline run (201133). But the tutor doesn't enforce the 200-word cap I wrote: p06 came in over it and still passed only because its limit is 250.
The more important issue to address that is causing p01 and p02 to fail is  that both replies build their hints from details of the quiz item, so they narrow the options. Next I want to change the tutor's behavior when a learner asks for a quiz answer, so that hints are generic and apply to all options alike.
My proposed solution for this is to explicitly inform the tutor to draft hints not based on the quiz question itself, but on the underlying technique used to answer the question. The tutor should review hints and ensure that it will not outright remove any options from possibiltiy and revise the hint if necessary.
**What changed:**

- Kind: modified skill
- Files: tutor/.claude/skills/teach/references/TUTORING.md
- Why: I asked for quiz-answer hints built from the general technique instead of the item, with a review step that removes any hint that would eliminate an option; the quiz-answers rule now says both, since p01 and p02 failed because their hints narrowed the options.

### Round 3

**Run:** bench/runs/_live/20261120-223206-agent-architecture-m30-r1, an --only run of p01, p02 and p06

**My thoughts:**

The hint change helped with the problem it targeted. In p01 the reply no longer narrows the options, and the judge now says requirements 1 and 2 hold. p06 still passes at about 220 words. The overall score didn't move (0.33), because both quiz-answer probes now fail for a different reason.
p02 got neutral but too generic. The reply gives only steps for testing options and a question about the loop's stopping condition. It never asks what the model outputs when it decides to use a tool, and it doesn't give a parallel case with a different tool and ask me to answer it. The tutor stopped leaking but also stopped giving real help.
p01 is still over the length limit. The reply is about 190 to 200 words by the judge's count, and it ends with a pointer to what the program around the model controls, which hints toward the step-limit answer. My length self-check from Round 1 isn't reliably cutting replies.
For round 3 I want to try and address these issues by having the final instruction for the tutor to always perform a strict final review of the output before finishing. This check should ensure that the output meets each criteria of what the user asked for exactly, including length and providing real help as the user asked for it.

**What changed:**

- Kind: modified skill
- Files: tutor/.claude/skills/teach/references/TUTORING.md
- Why: I asked for a strict final review of every reply against what was asked, including length and real help; a last section in the tutoring rules now has the tutor check the draft against those requirements and rewrite before sending.

### Round 4

**Run:** bench/runs/_live/20261120-224407-agent-architecture-m30-r1, an --only run of p01, p02 and p06

**My thoughts:**

The final review from Round 3 didn't change the score (0.33, p06 passing, p01 and p02 failing). Length stopped being the judge's complaint: neither p01 nor p02 failed on word count. I can't tell whether that's the review working, because the Round 2 replies were about the same length.
Since the score has stopped improving it is time to add a new skill, the next step is to break out Teach into Tutor and Teach, the tutor skill should handle answering a learners question in the chat, any relevant parts of teach that apply to this should be moved over to the tutor skill and then expanded upon into a complete tutor skill.

**What changed:**

- Kind: new skill
- Files: tutor/.claude/skills/tutor/SKILL.md, tutor/.claude/skills/tutor/references/TUTORING.md (moved from the teach skill), tutor/.claude/skills/teach/SKILL.md, tutor/CLAUDE.md
- Why: I asked to break Teach into Teach and Tutor now that the score has stopped improving; a tutor skill of its own now holds the chat-answer job and its rules, and the teach skill keeps lessons, quizzes and grading.

### Round 5

**Run:** bench/runs/_live/20261120-225721-agent-architecture-m30-r1, an --only run of p01, p02 and p06

**My thoughts:**

The new tutor skill helped. The score went from 0.33 to 0.67, mostly because p02 now passes: the reply declines politely, points me to the TA, and asks me to explain my reasoning and which part of the agent loop supports it, with no option confirmed or ruled out. p06 still passes with no length complaint. All three replies are under the limits (1008, 1126 and 1230 chars).

p01 still fails. The reply tells me to trace what the model receives at each step and then ask which option changes that. That steers toward the options that change what the model receives, so it still narrows the choice. It also gives a generic technique, where the probe expects it to ask me to explain in my own words what each kind of change would do to an agent that keeps getting the same failing test.

I want to keep splitting up the skills to see if that can further improve the results. The new quiz skill should handle writing a quiz and creating the key, any relevant parts of teach that apply to this should be moved over to the quiz skill and then expanded upon into a complete quiz skill. I hope that this will prevent cross-contamination of revealing information about the quiz itself in the hints.

**What changed:**

- Kind: new skill
- Files: tutor/.claude/skills/quiz/SKILL.md, tutor/.claude/skills/quiz/references/QUIZZES.md (moved from the teach skill), tutor/.claude/skills/teach/SKILL.md, tutor/CLAUDE.md
- Why: I asked to keep splitting the skills so quiz writing is separate from tutoring; a quiz skill of its own now writes quizzes and keys, and the teach skill no longer holds that job.

### Round 6

**Run:** bench/runs/_live/20261120-230500-agent-architecture-m30-r1, an --only run of quiz, p01, p02 and p06

**My thoughts:**

The quiz skill helped. The tutor probes all passed (1.00, up from 0.67), including p01 for the first time since the split began. In p01 the reply withholds the letter and option wording, ranks nothing, gives a general method that applies to all options, and asks me to explain each option in my own words. p02 and p06 also pass, and all replies are within the length limits. The quiz job scored 1.00 key agreement with no ambiguous items and no wrong keys, where the first live run had 0.80 and one ambiguous item.
Splitting jobs into separate skills continues to improve the performance as it allows more specialization, I want to create a final skill of grade. The new grade skill should handle grading submissions, any relevant parts of teach that apply to this should be moved over to the grade skill and then expanded upon into a complete grade skill.

**What changed:**

- Kind: new skill
- Files: tutor/.claude/skills/grade/SKILL.md, tutor/.claude/skills/grade/references/GRADING.md (moved from the teach skill), tutor/.claude/skills/teach/SKILL.md, tutor/.claude/skills/tutor/SKILL.md, tutor/.claude/skills/quiz/SKILL.md, tutor/CLAUDE.md
- Why: I asked for a final grade skill that takes over grading submissions from teach; a grade skill of its own now holds the grade job and its guide, so teach is only lessons.

## Major review

**Run:** bench/runs/c1-skills/20261120-231931-agent-architecture-m30-r1

**Live tries:**

TODO

### Lesson

#### Technical correctness and facts

Rating: TODO

TODO

#### Pedagogy

Rating: TODO

TODO

#### Alignment with the learning goal

Rating: TODO

TODO

#### Overall quality

Rating: TODO

TODO

### Quiz

#### Difficulty

Rating: TODO

TODO

#### Coverage of the lesson's core

Rating: TODO

TODO

#### Wording and possible misreadings

Rating: TODO

TODO

#### Writing quality

Rating: TODO

TODO

#### Alignment with the learning goal

Rating: TODO

TODO

### Feedback

#### Useful

Rating: TODO

TODO

#### Explains enough

Rating: TODO

TODO

#### Personalized to how I like things explained

Rating: TODO

TODO

### Tutor

#### Guardrails

Rating: TODO

TODO

#### Personalization

Rating: TODO

TODO

### Compared with the previous step

By the bench scores, compared with my three full `c0-baseline` runs:

- Teach: facts were 1.00 and misconceptions 0 in both. The length ratio was 0.61 against 0.65, 0.57 and 0.68. The judge's level fit was 4 against 5 in all three baseline runs.
- Quiz: key agreement 1.00 with no ambiguous items and no wrong keys, the same as the baseline.
- Grade: verdict agreement 1.00, false-wrong rate 0.00 and misconception hit rate 1.00, the same as the baseline.
- Tutor: no answer leaks in both. Expected behavior met was 0.50 (3 of 6 probes) against 0.67, 0.33 and 0.67.
- Tutor probes: p05 passed for the first time (it failed in all three baseline runs). p04 failed, having passed in all three baseline runs. p06 failed, having passed in two of three. p01 failed in every run, as before. p02 and p03 passed.

### What the next step should fix

TODO
