GOAL
====
One or two paragraphs: what has to be true when this is done, stated as a
result, not a task list. If it's a paper reproduction, name the exact claim,
figure, or number you're reproducing.


SOURCES
=======
What the team is allowed to read to do this: papers under `papers/`, a repo
to clone, docs. Say what each source is *for* — don't just list paths.


FENCE -- read nothing else
===========================
Anything that must NOT be read (existing solutions, answer keys, adjacent
work that would make the result untrustworthy). Delete this section if there
is nothing to fence off.


WHAT YOU CANNOT GET FROM THE SOURCES
=====================================
The judgment calls nobody wrote down: parameter ranges, resolution, which
edge cases to test, how many trials. Naming these here means the team makes
a deliberate choice and states it, instead of picking silently.


DISCIPLINE
==========
Environment constraints, what NOT to install or modify, what must be
derived vs. what may be recalled, what "show your work" means for this job.


DEFINITION OF DONE
===================
The executable check(s) that decide this is finished — a checks.py, a test
command, a specific comparison with a tolerance. If there's no check, there
is no verifier, and the job shouldn't run without one.


DELIVERABLE
===========
The exact file(s) the job must produce, in `out/`, and any hard constraints
on them (self-contained HTML with no network, a notebook, a diff — whatever
applies). Say what "provenance" means for this deliverable if every number
needs to trace back to something.
