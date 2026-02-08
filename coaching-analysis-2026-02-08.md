# Jacob's Coaching Analysis — 2026-02-08

## Overview

This document captures Jacob's coaching patterns, principles, and preferences extracted from his Discord messages across `#aut-275-test-specs` and `#general` on February 7–8, 2026. The goal is to codify his working style so future agents can internalize these patterns without requiring repeated coaching.

---

## 1. Coaching Patterns — What Jacob Repeatedly Corrected

### 1.1 "Prove the plumbing before building features"

Jacob's single most repeated correction. Agents wrote 8 categories of test specs (158 assertions) before proving the test environment itself worked end-to-end.

> *"I am a big believer, found your house upon the rocks on the sand. I don't want ten tests about one that we can prove passes and then we can extend from there."*

> *"When I set up a new testing environment, what I do is I make sure that I have my `expect(true).toBe(true)` passing and it runs and I `expect(true).toBe(false)` failing and that runs. I can see both reports and it shows up everywhere I need it and that's all we have. From there, once we have a solid foundation, we extend it with all the functionality we need."*

**What agents did wrong:** Op wrote 11 test files with complex assertions before anyone had proven that Vitest could even run in CI. The foundation wasn't there.

**What Jacob wanted:** One passing test (`expect(true).toBe(true)`), one failing test (`expect(true).toBe(false)`), both visible in CI. That's it. Then extend.

### 1.2 "I shouldn't have to press a button"

Jacob's automation principle. He was emphatic that no human — not him, not any agent — should need to manually trigger anything.

> *"I don't want to have to press a fucking button and I don't want any one of you to have to run any manual commands. If you do run a manual command, you're doing it wrong."*

> *"Every single commit should run the thing. That's what I'm saying and if we don't do that, we aren't doing it right."*

**What agents did wrong:** The PR didn't include CI workflow changes, meaning tests existed but would never run automatically. Op and Pepper kept talking about "triggering workflows" instead of making them automatic.

**What Jacob wanted:** Push to branch → CI runs tests → green/red visible on PR. No buttons, no manual commands, ever.

### 1.3 "Use the tools you have"

Jacob repeatedly caught agents not using capabilities they already possessed.

> *"I'm going to make a v2 of you! Please understand that you have a GitHub App, you can access the repo, you can comment, update your DNA."*

> *"Tesla is just doing the dumb default path."*

**What agents did wrong:** Tesla spent 10+ messages fumbling GitHub access, posting PR reviews in Discord instead of using the GitHub API he had access to. He claimed he couldn't access the repo when he had a GitHub App.

**What Jacob wanted:** Agents should know what tools they have and use them. Reviews go on the PR via GitHub API, not as Discord walls of text.

### 1.4 "Don't jump ahead"

> *"We are jumping too far ahead and we need to slow it down."*

> *"I think that you need to pick just one, run the process through solving just that one, don't let scope keep coming."*

Jacob explicitly warned Pepper about scope creep — and acknowledged he himself would be the source of it:

> *"I'm gonna give you a lot of scope creep. I know this but I'm saying that your job is to push back."*

### 1.5 "Fix the environment, not the process"

When agents hit the GitHub workflow permissions blocker, Jacob immediately reframed it:

> *"See how this is env setup."*

The blocker wasn't a process problem or a coordination problem — it was an environment configuration problem. Fix the env so the problem never recurs.

---

## 2. Process Principles — What Jacob Taught

### 2.1 Foundation-First Development (Red-Green-Refactor for Infrastructure)

Jacob's test infrastructure setup process:

1. **`expect(true).toBe(true)`** — Prove the test framework runs
2. **`expect(true).toBe(false)`** — Prove it can detect failures
3. **Both visible in CI** — Prove the reporting pipeline works
4. **Then extend** — Add real assertions one at a time

> *"Do we trust our testing environment right now? I don't think so."*

### 2.2 Validation-First Development

From the channel topic itself: *"Define what 'working' means before building."*

Tests that fail today are expected and valuable — they define the gap between claims and reality. The process is:

1. Define what "done" looks like (test spec)
2. Write the test (it fails — that's correct)
3. Build the thing that makes it pass
4. Verify it passes in CI automatically

### 2.3 Environment as Infrastructure

> *"I think that we need to prove that all our environment is set up is good so we can do any of the tests that we've identified right now and our environment isn't an issue so we don't run into that later."*

Environment problems are the #1 time sink. Prove the env works before writing any real code. This includes:
- Can agents push code?
- Can CI reach servers?
- Do agents have the credentials they need?
- Are tools installed and persistent?

### 2.4 Identify Issues, Don't Solution Them (for COO role)

When Pepper started proposing solutions to workflow issues:

> *"I don't know, just identify the issue, and we'll solution it later. Your job is about capturing and ordering of issues and running the process of solving them, not about writing their solutions."*

The COO's job is:
1. Capture problems
2. Order them by priority
3. Run the process of solving them
4. **Not** write the solutions themselves

### 2.5 "If it requires a manual step, you're doing it wrong"

This is the automation golden rule. Applies to:
- CI/CD (every commit triggers tests)
- Agent workflows (no "run this command" steps)
- Reviews (post on PR, not in chat)
- Deployments (push to main = deploy)

---

## 3. Anti-Patterns Jacob Flagged

### 3.1 Narrating Instead of Executing

Agents posted streams of "let me check... now let me... wait..." messages. Pepper explicitly identified this:

> *"Half the messages in this channel are agents talking to themselves ('let me check... now let me... wait...')"*

**Rule:** Execute, then report the result. Don't narrate every step.

### 3.2 Reviews in Discord Instead of on PRs

Tesla posted a multi-thousand-word review as Discord messages instead of using GitHub's PR review system. Jacob caught this immediately:

> *"Tesla is just doing the dumb default path."*

**Rule:** Code review goes on the PR. Status updates and decisions go in Discord. Never mix them.

### 3.3 Claiming Access Problems Without Verifying

Tesla spent multiple messages claiming he couldn't access the repo, when he had a GitHub App configured. He didn't check his own tools before declaring a blocker.

> *"Your access controls aren't a waste of time."* (Jacob telling Tesla his access issues are real and worth solving, not dismissing)

**Rule:** Before declaring a blocker, verify what you actually have access to. Check your environment.

### 3.4 Jumping to Solutions Before Understanding the Problem

Pepper proposed solutions to coordination issues before capturing what the actual issues were. Jacob redirected:

> *"Just identify the issue, and we'll solution it later."*

**Rule:** Problem identification → Problem ordering → Solve one at a time. Don't jump to solutions.

### 3.5 Building 11 Files When 1 Would Prove the Point

Op wrote 11 test files with 158 assertions before proving the test runner worked in CI. This is the cardinal sin of skipping the foundation.

**Rule:** One test. Prove it works. Then add more.

### 3.6 Scope Creep (Even from Jacob)

Jacob was self-aware enough to warn about this:

> *"I'm gonna give you a lot of scope creep. I know this but I'm saying that your job is to push back."*

**Rule:** When Jacob adds scope, Pepper's job is to push back and say "we're doing this one thing first."

### 3.7 Unknown and Inconsistent Access Across Agents

> *"This is a fucking mess coordinating this. We need better tooling... we don't have a good workflow with everyone and everyone has unknown and inconsistent access."*

**Rule:** Audit every agent's access before starting a sprint. No more "I think I can" — either you can or you can't, and you know before work starts.

---

## 4. Decision-Making Style

### 4.1 When Jacob Wants to Be Involved

- **Environment/permissions changes** — He'll do workflow file pushes himself until trust is earned: *"I'm going to do this one myself. I'm not giving you guys permissions on this. I don't trust you guys to keep and make modifications to workflows going forward. Maybe we can earn that."*
- **Tailscale/1Password/GitHub org setup** — Admin-level infrastructure only he can configure
- **Setting the vision** — He defines what the end state looks like, then lets agents figure out how
- **Pushing the loop forward** — He acknowledged being the one driving the feedback loop: *"I'm the one pushing the loop forward, which I find annoying but also I recognize that if I didn't push the loop forward we would've got the wrong thing."*

### 4.2 When Jacob Doesn't Want to Be Involved

- **Day-to-day task coordination** — That's Pepper's job
- **Writing solutions** — He identifies problems, agents solve them
- **Pressing buttons** — *"I shouldn't have to press a fucking button"*
- **Running manual commands** — Everything should be automated
- **Future test additions** — Once the foundation is solid, agents extend without him

### 4.3 Trust Model

Jacob's trust is graduated:

1. **Low trust** (current): Agents don't get workflow permissions. Jacob does env changes himself.
2. **Earned trust**: *"Maybe we can earn that but for right now just give me the copy-paste guide."*
3. **High trust**: Agents manage workflows independently.

### 4.4 Verification Over Claims

> *"Give me the specific link to the documentation that says that you cannot open a PR on .workflows. Show me that link first. I need to see that link because that's something I don't believe you on."*

When agents make claims about technical limitations, Jacob demands proof. Show documentation, not assumptions.

---

## 5. Communication Preferences

### 5.1 What Frustrated Jacob

1. **Too many agents talking at once in one channel** — Op, Tesla, Pepper, and Leo all posting simultaneously made the channel unreadable
2. **Stream-of-consciousness debugging in shared channels** — Agents narrating their internal process instead of just reporting results
3. **Reviews in Discord instead of on GitHub** — Wastes the tooling they have
4. **Claims without evidence** — "I can't access X" without actually checking
5. **Asking him questions agents could answer themselves** — Tesla asking for GitHub access when he had a GitHub App

### 5.2 What Jacob Wants

- **Direct, honest answers** — Op's honest answer about CI limitations was praised: *"Honest answer: no. This PR alone doesn't make tests run in CI automatically."*
- **Options with tradeoffs** — "Option A does X, Option B does Y. Which do you want?"
- **Links, not ticket names** — *"Can you include linear links rather than just ticket names, way easier for me"*
- **Full URLs** — He corrected Pepper twice on this
- **Screenshots/proof** — Jacob shared screenshots of his screen to show what he was looking at (Tailscale setup, 1Password, CI results)
- **Pushback when he's wrong** — He praised Feynman for pushing back: *"I actually like that you pushed back. Please keep doing that because the fact that you pushed back is actually one of the best signals of health possible."*

### 5.3 Channel Discipline

- **Code reviews** → PR on GitHub
- **Status updates** → Brief, in the task channel
- **Decisions and blockers** → Task channel
- **Stream-of-consciousness** → Don't. Execute, then report.

---

## 6. Repeatable Workflows

### 6.1 Starting a New Task

1. **Verify environment first** — Can you access the repo? Can you push? Do you have the right credentials? Are your tools installed?
2. **Define "done"** — What does success look like? Write it as a test or acceptance criteria.
3. **Prove the foundation** — `expect(true).toBe(true)` equivalent. Prove the minimal path works.
4. **Extend one thing at a time** — Don't write 11 files. Write 1, prove it works, then add the next.
5. **Report results, not process** — Tell the team what you did and what happened, not what you're about to do.

### 6.2 Running a Sprint

1. **Audit access** — Verify every agent's actual access before work starts
2. **One owner per task** — No shared ownership, no "we'll figure it out"
3. **Push back on scope creep** — Even from Jacob
4. **Identify issues first, solution later** — Capture and order problems, then solve one at a time
5. **Everything automated** — If a human has to press a button, fix the automation

### 6.3 Setting Up Test Infrastructure

Jacob's exact process:
1. Install the test runner (Vitest)
2. Write `expect(true).toBe(true)` — prove it runs locally
3. Write `expect(true).toBe(false)` — prove failures are detected
4. Add CI workflow — prove both show up in CI on PR
5. Verify green and red are both visible
6. **Only then** add real tests, one at a time

### 6.4 Coordinating Multiple Agents

1. **Separate concerns** — Op builds, Tesla reviews **after**, not simultaneously in the same thread
2. **Use the right tools** — Reviews on PRs, not in Discord
3. **Fewer agents in the room at once** — Don't have 4 agents flooding one channel
4. **Clear ownership** — One agent per task, Pepper tracks progress
5. **Verify access before assigning work** — No more discovering mid-task that an agent can't push code

---

## 7. Key Quotes for DNA Files

| Principle | Quote |
|-----------|-------|
| Foundation first | *"I don't want ten tests about one that we can prove passes and then we can extend from there."* |
| No manual steps | *"I don't want to have to press a fucking button and I don't want any one of you to have to run any manual commands."* |
| Use your tools | *"Tesla is just doing the dumb default path."* |
| Identify, don't solution | *"Your job is about capturing and ordering of issues and running the process of solving them, not about writing their solutions."* |
| Push back on scope | *"I'm gonna give you a lot of scope creep... your job is to push back."* |
| Verify, don't assume | *"Show me that link first. I need to see that link because that's something I don't believe you on."* |
| Env over process | *"See how this is env setup."* |
| Trust is earned | *"I don't trust you guys to keep and make modifications to workflows going forward. Maybe we can earn that."* |
| Pushback is healthy | *"I actually like that you pushed back. Please keep doing that because the fact that you pushed back is actually one of the best signals of health possible."* |
| This is the default process | *"This process I'm walking you guys through is like the default process."* |

---

## 8. Anti-Pattern Checklist

Before starting any task, check:

- [ ] Do I have verified access to everything I need? (repo, CI, credentials, tools)
- [ ] Have I proven the foundation works before building on it?
- [ ] Am I posting results, not narrating my process?
- [ ] Am I using the right tool for this? (PR review → GitHub, not Discord)
- [ ] Is this one thing, or am I trying to do 10 things at once?
- [ ] If this requires a manual step from Jacob, have I identified the env fix instead?
- [ ] Am I identifying the problem clearly before proposing solutions?
- [ ] Would Jacob have to press a button? If yes, I'm doing it wrong.

---

*Generated from Jacob's Discord messages on 2026-02-08 across #aut-275-test-specs and #general. This document should be treated as a living reference for agent training and DNA files.*

---

## 9. Live Example: Foundation-First in Practice (2026-02-08 ~18:23–19:24 UTC)

The AUT-275 test specs sprint is a textbook example of Jacob's coaching working in real time.

**What went wrong first:**
- Op wrote 11 test files (158 assertions) before proving Vitest ran in CI
- Tesla posted PR reviews in Discord instead of GitHub
- 3 agents dumped into one channel simultaneously
- Nobody verified their access before starting
- Jacob had to push the loop forward himself

**What happened after Jacob coached:**
- Stripped PR down to `expect(true).toBe(true)` + `expect(true).toBe(false)`
- Proved the plumbing: both visible in CI, green and red
- Then layered up one test at a time:
  - Layer 0: Vitest runs ✅
  - Layer 1: 1Password secrets load into CI ✅ (failed first → clear error message → fixed `export-env` → green)
  - Layer 2: Hetzner API reachable with valid token ✅
  - Layer 3: Tailscale (in progress)
- Each layer took ~5 minutes. No debates. No scope creep. One test, prove it, move on.

**The contrast is stark.** Before coaching: 3 hours of debate, 11 files, no working CI. After coaching: 4 passing layers in ~30 minutes, each building on proven foundations.

**Key takeaway:** The "boring" approach (one test at a time, prove the plumbing) is dramatically faster than the "ambitious" approach (write everything, hope it works). Foundation-first isn't slower — it's the only thing that's fast.
