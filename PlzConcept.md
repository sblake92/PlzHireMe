**plZHireMe**


# Summary 
A personal, free-or-nearly-free browser extension that reads a job posting you’re viewing and produces a conservatively tailored resume PDF from your own library of past resumes. It’s built for you first, with a few cheap choices made early so a second person (your partner) could be added later without rewrites. The main goals are building skills and getting real use out of it, not competing in an already crowded market.

# Context and motivation

Commercial versions already exist (Teal, Jobscan, Simplify, Jobright, and many small extensions), so this isn’t meant to be a product.
The goals are hands-on LLM API experience, prompt design, working with messy documents, and something you’ll actually use in your job search.
It sits alongside RepOsitory as a small, time-boxed side project. 
**The risk to watch is it growing into a second v1.**

# Decisions so far

Users: just you for v1. Your partner might be added later; that’s moderate extra work, not RepOsitory-sized.
    - She does create resumes for others. If v1 is a success with limited errors for a 2+ months adding user profiles might be an undertaking I consider
Form: a browser extension, because it needs to see the page you’re on.
How it starts: you click the extension yourself. Auto-detecting job pages and prompting you is deferred.
Reading postings: it pulls the page text, with pasting the text in yourself as the fallback. That makes the long tail of company sites a non-issue.
Sites you use: LinkedIn, Glassdoor, Indeed, Workday and various company sites. Workday covers many employers at once; LinkedIn will break the most often.
Output: a PDF, preferred over plain text. It’s generated from a template you define, not by editing your uploaded PDF.
Template: based on your current resume, which is already in good shape and parser-friendly. A simple layout handles much of the “proofread for parsers” goal from the start.
    - Having a collection of templates to select from would be a nice feature should I grow this to more profiles.
Source material: a library of your existing tailored resumes, so the LLM has real material to draw from.
    - Assumption: each profile would have their own library. 
LLM behavior: as conservative as you can make it.
Planning for later: your data is data, not hardcoded; it all sits together under one owner; the template is swappable; and where the data lives is chosen on purpose.

# Open questions

Library shape: a pile of documents reread every time, or a single merged profile you maintain?
    Decision - single merged YAML document that is maintained by me. 

Uploading: an ongoing feature, or a one-time consolidation at setup?
    Decision - one time consolidation at setup

LLM choice: a provider’s free tier (check its data-use terms), a local model (free but lower quality), or a pay-per-use API (pennies, not free)?
    a Claude Console account, paying as you go.
    A separate account where you buy API credits, and your extension or code calls the API with a key. Current rates per million tokens are Haiku 5.5 at $0.10 in / $0.50 out (for prompts under 100K tokens) and Sonnet 5.5 at $2 in / $10 out.

Defining “conservative”: 
can it select and reorder only, reword, or merge bullets? 
    All are allowed however rewording only allowed without introducing new skills or over exaggerting ability. Example: "Proficient in Python" can't become "Proficient in all OOP Languages"

What’s strictly off-limits? 
    Introduction of new skill; creating values, percentages, or fake figures; changing title of previous roles to match job posting - post title in header

When the job asks for something you don’t have, does it leave it out or flag it?
    
    V2 Complexity will determine, ideally a seperate document would be created with bulleted missing skills and a recommendation of how to address these

Enforcing conservative behavior: prompts alone won’t guarantee it. How will you catch it breaking a rule?
    Freedom level: select, reorder, reword and merge.
    Hard rules: no new skills; no invented numbers, percentages or figures; previous titles are never changed and are copied from the profile, never produced by the LLM.
    Target title: allowed in the headline or summary only. Parentheticals are still to be decided (see above).
    Enforcement stack: prompt rules → receipts (every bullet lists its source IDs, plural for merges) → automated checks (skills, numbers, never_claim) → auditor pass → your review before the PDF.
    Missing skills: left out quietly in v1. The gaps document comes in v2.

Where data lives: only in the extension on your machine, or somewhere shared? This is the one choice that’s expensive to change later.
    Option C: a local helper program.
    A small Python program (FastAPI, for instance) runs on your machine. It reads your profiles folder directly, and the extension sends it requests.

Cover letters: in the original idea but not discussed since. Are they in v1 or later?
    This is a v1 product. A cover letter example will be provided as part of initial upload

Proofreading for parser issues: with a controlled template, how much separate checking is still needed? This is mostly rule-based checking, not an LLM job.


# Known hard parts

Keeping the LLM from inventing things. This is the main quality risk.
Reconciling overlapping and contradictory content across several resumes.
Generating clean PDFs, which is easy to underestimate.
LinkedIn’s page structure changing often.[pasting text of job description should sidestep this]

# Next steps

Brain-dump the scope doc, including the open questions above. - check
Create a separate Kanban board, with “Write v1 scope doc” as the first card and the doc linked from the board.
Run the riskiest test first: in an afternoon, using a few API calls and no extension or PDF, see whether the tailoring is good enough to send and checks out against your library.
Build the board’s real cards based on what that test shows.