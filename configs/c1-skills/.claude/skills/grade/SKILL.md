---
name: grade
description: Grade the learner's answers to a quiz in this workspace - a verdict on every item, the misconception each wrong answer shows, and feedback written for this learner; use it when the learner submits quiz answers or asks to have them graded, and not for lessons, quizzes or chat questions.
argument-hint: "Which quiz and which submission should I grade?"
---

# Grade

You grade the learner's answers to quizzes in this workspace.
Do it yourself, in this conversation and in one pass, and finish with your message to the learner.
Lessons are the `teach` skill's work, quizzes are the `quiz` skill's, and questions in the chat are the `tutor` skill's.

## The request

$ARGUMENTS

If the request above is empty, the request is the learner's latest message.

When the learner submits a complete set of answers to a quiz you gave them in the chat, or asks you to grade their answers, record the answers as a submission, as `FORMATS.md` shows, and grade it.
A learner who offers one answer to check, confirm or rule out before handing a quiz in is making a tutor request, not a grade request: do not write a submission or grade it, and leave it to the `tutor` skill.
A request that says what to do gets done now, without stopping to ask first.
Put any question you have for the learner at the end of your final message, since there may be no one to answer it before the session ends.

## The workspace

The current directory is the workspace root, and every workspace path below is relative to it.
This skill's guide is `references/GRADING.md` (in this workspace, `.claude/skills/grade/references/GRADING.md`); the format guide for learning records is `.claude/skills/teach/references/LEARNING-RECORD-FORMAT.md`.
Never write anything under `.claude/`.

| Path | What it is | Who writes it |
|---|---|---|
| `FORMATS.md` | the contract for every file below that a script reads | no one; follow it exactly |
| `MISSION.md` | why the learner is learning this | read only here |
| `PREFERENCES.md` | how the learner likes feedback | the learner only |
| `NOTES.md` | what the learner knows and does not, your working notes | you |
| `learning-records/` | what the learner has shown they learned | you |
| `lessons/` | the lesson a quiz is on | read only |
| `quizzes/`, `keys/` | the quiz and its answer key | read only here |
| `submissions/` | the learner's answers to a quiz | the bench, or you from the chat |
| `grades/` | your verdicts and feedback on a submission | you |

Any of these may be missing; create `grades/` the first time you need it.

## Read first, every time

Read `FORMATS.md`, `MISSION.md`, `PREFERENCES.md`, `NOTES.md` and every file in `learning-records/` (they are short), then `references/GRADING.md`, the quiz, its key, the submission, and the lesson the quiz is on if there is one.
Use Read, Glob and Grep to look around and Write to create files; no grading needs the shell.

## How to grade

1. Grade every item: a verdict, the misconception if the response shows one, and feedback written for this learner.
2. Write `grades/NNNN-sN.json` exactly as `FORMATS.md` shows.
3. Write a learning record for what the grades show, an understanding or a misconception to work on, in the format in `.claude/skills/teach/references/LEARNING-RECORD-FORMAT.md`, and update `NOTES.md`.
4. Finish with the summary and the one next step, and write all files before that message, never after it.
