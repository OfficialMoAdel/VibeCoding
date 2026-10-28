---
name: premortem
description: "Run a premortem on any plan, launch, product, hire, strategy, or decision. Assume it already failed 6 months later and work backward to find every reason why. Produce a revised plan with the blind spots exposed. REQUIRED TRIGGERS: 'premortem this', 'premortem my', 'run a premortem', 'what could kill this', 'stress test this plan', 'what am I missing here', 'find the blind spots'. STRONG TRIGGERS: 'what could go wrong', 'am I missing something', 'poke holes in this', 'where will this break', 'devil’s advocate'. DO NOT trigger on simple feedback requests, factual questions, or LLM Council requests. DO trigger when someone has a plan or commitment where the cost of being wrong is high."
---

# Premortem

A premortem is the opposite of a postmortem. Instead of figuring out what went wrong after something fails, you imagine it has already failed and figure out why before you begin.   

The method comes from psychologist Gary Klein. He published it in Harvard Business Review. Daniel Kahneman (the Nobel Prize-winning psychologist behind *Thinking, Fast and Slow*) called it his most valuable decision-making technique. Google, Goldman Sachs, and Procter & Gamble use it before major decisions.   

The core idea: when you ask people, "what could go wrong?", they give cautious and ambiguous answers. When you say, "this already failed, tell me why", the brain shifts into narrative mode and produces much more specific, creative, and honest reasons. Researchers at Wharton and Cornell called this "prospective hindsight" and found that it significantly increases the ability to identify causes of future outcomes.   

Why this matters for AI-assisted decisions: Claude tends toward helpful and optimistic responses. If you ask, "is this a good plan?", it will find reasons to say yes. The premortem breaks that pattern by forcing the frame to become "this is dead, explain how it died." Claude stops looking for reasons your plan will work and starts explaining how it unraveled.   

---

## when to run a premortem

Good targets for a premortem:
- A product or feature you are about to build
- A launch plan with money or reputation at stake
- A pricing or business model change
- A hire you are about to make
- A strategy or positioning pivot
- A partnership or deal you are evaluating
- Any commitment where the cost of being wrong is high   

Bad targets for a premortem:
- Vague ideas without any concrete plan yet (help them plan first, then run the premortem)
- Questions with a single correct answer (just answer them)
- Creative feedback requests on a draft (that is editing, not a premortem)
- Decisions that are already made and irreversible (a premortem is only useful when you can still change course)   

---

## context gathering (the minimum required)

A premortem is only as good as the context it runs on. Vague information produces vague failure scenarios that help no one. Before running the premortem, you need to reach a minimum context threshold.   

### step 1: look for existing context

Before asking the user anything, look for context that is already available:   

**A. The current conversation.** The user may already have been discussing a plan, launch, product, or decision earlier in this session. Read the conversation and extract whatever is relevant.   

**B. The workspace.** Quickly scan for files that may contain relevant context:
- `CLAUDE.md` or `AGENTS.md` (business context, preferences, constraints)
- Any `memory/` folder (audience profiles, business details, past decisions)
- Any files the user explicitly referenced or attached
- Any project files, briefs, or plans related to what is being premortemed   

Use `Glob` and quick `Read` calls. Do not spend more than 30 seconds on this. You are looking for the key files that will anchor the failure scenarios in reality.   

### step 2: assess context sufficiency

After scanning, check whether you have enough to run a useful premortem. You need three things:   

1. **What is it?** — A clear understanding of what is being premortemed (a product, a launch, a hire, a pricing change, a strategy). You should be able to describe it back to the user in one sentence.   

2. **Who is it for / who is affected?** — The audience, customer, team, stakeholders. Failure scenarios depend heavily on who is involved.   

3. **What does success look like?** — What outcome is the user hoping for? Failure is defined by inverting success. If you do not know what success means, you cannot define what failure means.   

### step 3: fill the gaps conversationally

If you have all three, proceed directly to the premortem. Do not ask unnecessary questions.   

If one or more are missing, ask for the most important missing piece first. One question at a time. After each answer, assess whether you now have enough. Keep asking until you reach the threshold, but never ask more than necessary.   

Examples of focused context questions:
- "What exactly are you about to launch/build/decide?" (if you do not know what it is)
- "Who is this for?" (if you know the plan but not the audience)
- "What would success look like here?" (if you know the plan and audience but not the success criteria)   

The goal is to reach the minimum as quickly as possible without making the user feel like they are filling out a form. Conversational, not interrogative. If you can infer an answer from context, do that instead of asking.   

---

## how a premortem session works

### step 1: set the frame

Once you have enough context, explicitly set the premortem frame. Something like:   

"Okay, I have enough context. Let’s run the premortem. The premise is: it is 6 months later. [The plan/launch/decision] has failed. It is over. We are looking backward to understand what went wrong."   

This framing matters. It shifts the mode from "evaluate this plan" (which triggers polite responses) to "explain why this died" (which triggers honest and specific failure identification).   

### step 2: generate failure reasons (raw premortem)

Run the raw premortem as one complete analysis. No predefined categories, no lenses, no constraints. Just the basic Klein method:   

"This plan has failed 6 months later. Generate every genuine reason it could have died. Be exhaustive. Be specific. Ground each reason in the real details of the plan. Do not pad with weak reasons and do not stop early if there are more."   

The output should be a complete list of failure reasons, each expressed in 1–2 sentences. Be honest and exhaustive. Some plans may have 4 genuine failure modes. Others may have 9. The number should be whatever is real for this specific plan.   

Each failure reason should be:
- Specific to this plan (not generic advice that applies to anything)
- Grounded in real details the user provided
- A genuine threat (not a minor inconvenience or an extremely unlikely edge case)   

### step 3: deep analysis agents (one per failure reason, all in parallel)

Take each failure reason from step 2 and launch one sub-agent per reason, all in parallel. Each agent takes its assigned failure reason and analyzes it independently in depth.   

**Sub-agent prompt template:**

```text
You are an investigator in a premortem analysis. You have been assigned one specific failure reason to analyze deeply.

The plan:
***
[full context: what it is, who it is for, what success looks like, plus any relevant context from the workspace]
***

PREMORTEM FRAME: It is 6 months later. This plan has failed.

YOUR ASSIGNED FAILURE REASON: [the specific failure reason from step 2]

Your job is to go deep on this failure. Write the story of how it actually unfolded. Be specific. Use details from the plan. Make it feel real, like a case study of something that actually happened.

Your output must include:

1. THE FAILURE STORY: A 2–3 paragraph narrative of how this specific failure unfolded. Use details from the plan. Name specific moments where things went wrong and why.

2. THE UNDERLYING ASSUMPTION: The single thing the user was taking for granted that made this failure possible. State it in one sentence.

3. EARLY WARNING SIGNALS: 1–2 concrete, observable signals the user could watch for that would indicate this failure mode is beginning to develop. These should be things that can actually be seen or measured, not vague feelings.

Keep the total response under 300 words. Be direct. Do not hedge. Do not soften it.
```   

### step 4: synthesis

After all agents complete, read each deep analysis and produce the synthesis:   

**PREMORTEM REPORT**

1. **Most Likely Failure** — Which failure scenario is most likely given what you know about the plan? Why? This is the one the user should focus on first.   

2. **Most Dangerous Failure** — Which failure scenario would cause the most damage if it happened, even if it is less likely? This is the one worth safeguarding against.   

3. **Hidden Assumption** — Across all the failure analyses, what is the most important assumption the user is making and likely has not questioned? This is often where the real value of the premortem lives: what is so obvious to the user that they forgot it was an assumption.   

4. **Revised Plan** — Based on the failure scenarios, what specific changes would make the plan more resilient? Be concrete. Do not say "consider your pricing." Say "test the price at $X with 20 people before committing publicly." Each revision should map directly to a specific failure scenario.   

5. **Pre-Launch Checklist** — 3–5 specific things the user should verify, test, or implement before proceeding. Each one should prevent or detect one of the failure modes identified.   

### step 5: generate the premortem report

Generate a visual HTML report and save it in the user’s workspace.   

**Filename:** `premortem-report-[timestamp].html`   

The report should be a single self-contained HTML file with inline CSS. Design principles:
- Dark background (`#0a0e1a` or similar), clean typography, easy to scan
- The synthesis section (most likely failure, most dangerous failure, hidden assumption, revised plan, checklist) should be shown prominently at the top because that is what most people will read first
- One visual card per failure reason showing the deep analysis. Each card should display the failure reason as the title, the failure story, the underlying assumption, and early warning signals. Use distinct accent colors for each card so they are visually scannable
- A clear visual severity/probability indicator for each failure mode
- The rotating visual: show the number of agents that were run and their findings in a grid or card layout so the user can see the full scope of the premortem at a glance
- Footer with timestamp and what was premortemed   

Open the HTML file after generating it.   

### step 6: save the transcript

Save the full premortem transcript as `premortem-transcript-[timestamp].md` in the same location. This includes:
- The context that was gathered (what, who, success criteria)
- The raw premortem failure reasons
- All deep agent analyses
- The full synthesis   

---

## output format

Each premortem session produces two files:

```text
premortem-report-[timestamp].html    # visual report for scanning
premortem-transcript-[timestamp].md  # full transcript for reference
```   

The user sees the HTML report first. The transcript is available if they want to go deeper into the reasoning behind each failure scenario.   

Also provide a concise chat summary: the most likely failure, the hidden assumption, and the single most important revision to the plan. Maximum three sentences. The report contains all the details.   

---

## example: premortem for a product launch

**User:** "premortem this: I’m about to launch a $297 live workshop on how to use Claude Cowork for marketing teams. 50 seats. Targeted at marketing directors at companies with 10–50 employees."   

**The raw premortem identifies 6 failure reasons:**
1. Marketing directors at companies this size need approval to spend $297 on professional development, adding friction you did not account for
2. "Claude Cowork for marketing" is a tool-first pitch in a market where most directors are still deciding whether AI is relevant to them
3. The audience that actually buys may be solopreneurs, not team directors, creating a mismatch between the content and the attendees
4. Building a workshop for marketing teams requires demo environments with realistic marketing data and multi-user setups, which takes 5 weeks of preparation, not the 2 you budgeted
5. If 60% of attendees are solopreneurs, your testimonials and case studies will not resonate with the marketing director audience you need for future cohorts
6. At $297 with 50 seats, the maximum revenue is $14,850, which may not justify the preparation time compared with other revenue opportunities   

**6 agents then analyze each reason independently, producing failure stories, underlying assumptions, and early warning signals.**   

**Synthesis:** The most likely failure is audience mismatch: you are targeting people who need approval to spend $297, which adds friction you did not account for. The most dangerous failure is attracting solopreneurs instead of team directors, because your case studies and testimonials will not resonate with the actual target buyer for future cohorts, compounding the problem over time. Hidden assumption: you assume "marketing directors at 10–50 person companies" is a reachable audience, but these people do not identify that way and are not all in the same places. Revised plan: run a $47 pilot session for 20 people first. Use that to identify whether your real buyers are team directors or solopreneurs, then build the full workshop for whoever actually shows up.   

---

## important notes

- **Always launch all failure agents in parallel.** Sequential execution wastes time and lets earlier responses influence later ones.
- **Always set the premortem frame explicitly.** "This has already failed" is the psychological mechanism that makes this work. Without it, the analysis turns back into polite risk evaluation instead of honest failure identification.
- **Be exhaustive, but do not pad.** Find every genuine failure reason. Do not stop at 3 if there are 7. But do not force 7 if there are only 3. The number should be whatever is real for this specific plan.
- **The synthesis is the product.** Most users will read the synthesis and skim the individual failure cards. Make the synthesis specific and actionable.
- **Do not soften it.** The point of a premortem is to tell the user what they do not want to hear before reality does. If a plan has serious problems, say so directly.
- **The revised plan must be concrete.** Do not say "consider testing your pricing." Say "run a $47 pilot with 20 people before committing to the full $297 workshop." Each revision should be something the user can actually do this week.
- **Respect the minimum context threshold.** Running a premortem with insufficient context produces generic failures that waste the user’s time. It is better to ask one more question than to produce a bad premortem.
- **This is not LLM Council.** Council gives multiple perspectives on a decision right now. Premortem sends Claude into the future where the decision has already failed and works backward to explain why. Different psychological mechanism, different result. If the user seems to want multiple perspectives rather than failure analysis, suggest council instead.   
