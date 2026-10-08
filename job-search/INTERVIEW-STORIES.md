# Interview Story Pack

> Six STAR stories built from work you actually did. Each has a `⚠️ YOU MUST FILL` line — those are
> facts only you know. **Do not guess them.** "I don't remember the exact number, but the shape of it
> was..." is a perfectly strong interview answer. A fabricated number is not.
>
> Practice these out loud. The goal is not memorising a script — it is being able to go three
> follow-ups deep, which you can, because you lived it.

---

## Story 1 — "Tell me about a technically hard problem you solved"

**Use for:** senior-signal questions, security roles, systems design screens.
**Based on:** the session-token versioning work.

**Situation.** Our platform issues session tokens to every logged-in player across all brands. The
team needed to introduce a new cryptographic algorithm for those tokens. The obvious problem: tokens
already in the wild were produced by the old scheme, and we could not log out every active player.

**Task.** I had to make the token format forward-compatible so the crypto scheme could be rotated
later without a flag-day migration.

**Action.** I introduced an explicit version marker at the front of the token value, and changed the
token reader so it inspects that marker first: if the value carries a version marker, the reader
strips it and reads the remainder with the algorithm that version maps to; if there is no marker, it
falls back to the legacy path. That gave us a discriminator we never had before, so a second version
can be introduced later and both can coexist during the rollout window.

**Result.** The platform gained a safe rotation path for a security-critical primitive with no forced
re-authentication. ⚠️ **YOU MUST FILL:** what state the change was in when you left — "it was in
review" is an honest and normal answer. Shipping state is not a weakness.

**Expect follow-ups:** Why a prefix rather than a header or a separate field? (Answer: tokens are
opaque strings passed through many clients; changing the envelope would have broken consumers.) What
stops someone forging a token with a valid-looking version marker? How do you eventually drop support
for the old version, and how do you know when none of them are left in circulation? — *Have an
opinion on each; you don't need to have shipped it.*

*Keep it at this level of description. Don't reproduce the literal token format in an interview,
a take-home, or anything written down — describing the pattern demonstrates the same skill without
handing out a live system's internals.*

---

## Story 2 — "Tell me about a production incident"

**Use for:** on-call, reliability, ownership questions.
**Based on:** the thread-unsafe MD5 calculator fix, plus the P2 wallet degradation.

**Situation.** We had a production incident where requests were failing intermittently and
unreproducibly in lower environments. The root cause turned out to be a shared MD5 calculator
instance being used concurrently — .NET's hash algorithm objects are not thread-safe, so under
concurrent load the internal state was being corrupted between callers.

**Task.** `[Be precise about your role here.]` ⚠️ **YOU MUST FILL:** you were the *reviewer* on this
fix, not the author — another engineer on the team wrote it. **Say that.** "I reviewed the fix and
worked the incident" is a completely respectable answer and it is the truth. If you want an incident
you owned outright, use the P2 below instead.

**Alternative Situation (yours to own).** August 2026 — a P2 where wallet betslips and deposits
degraded across eight European markets, traced back to a recent change.

**Action.** `[Your actual steps.]` ⚠️ **YOU MUST FILL** from memory: how you were paged, what you
checked first (log search in Kibana? the internal health dashboard?), how you correlated it to the change, who
you escalated to, whether the fix was a rollback or a forward fix.

**Result.** Incident closed. ⚠️ **YOU MUST FILL:** rough time-to-mitigate if you remember it.

**The transferable point to land:** "Intermittent and unreproducible in lower environments is a
signature. It usually means concurrency, caching, or a load-dependent path — so I stop trying to
reproduce and start reading the code for shared mutable state."

---

## Story 3 — "Tell me about working with ambiguity / many stakeholders"

**Use for:** collaboration, seniority, communication questions.
**Based on:** the 10 releases you cut.

**Situation.** I owned release management for our platform on a rotating basis — 10 releases across
the core API platform, the authentication service and a shared static-data service, including a
hotfix and releases that had to go out under a change freeze.

**Task.** A release on this platform is not a merge button. Multiple teams land changes into one
version; the change goes to regulated markets; a freeze means you need justification to ship at all.

**Action.** I ran the full checklist each time: assembling and verifying the changelog, bumping the
version, validating what was actually in the branch versus what the stories claimed, getting the
release through review, merging back to master so the next cycle started clean, updating each story
with the shipped version, and notifying the stakeholders and original requestors that their change
was live.

**Result.** 10 releases shipped. ⚠️ **YOU MUST FILL:** any release that went wrong and what you
changed afterwards — interviewers love this far more than a clean record. If none went wrong, say so
plainly.

**The transferable point:** "The hotfix one taught me the most — when you ship outside the normal
train you lose the safety of the normal process, so the discipline has to come from you."

---

## Story 4 — "Tell me about improving code quality or paying down debt"

**Use for:** craft questions, "what would you change about your last codebase."
**Based on:** sync→async refactors, WCF async regeneration, interceptor unit tests, Fortify→Checkmarx.

**Situation.** Our platform was a long-lived ASP.NET Core service with a lot of history — synchronous
database calls, synchronous WCF contracts, and a compliance interceptor pipeline that was
business-critical but thinly tested.

**Task.** Over time I picked up several threads of this.

**Action.** Three concrete pieces:
1. **Async.** I regenerated the entire WCF contract surface as async-only, removed the synchronous
   `Execute` and `ExecuteScalar` methods from our SQL invoker abstraction, and converted account
   publishing and CRM segmentation to async. Removing the sync overloads was the important part —
   leaving them available means the debt comes back.
2. **Tests.** I added unit-test coverage to the interceptor pipeline, which is where every
   jurisdiction's compliance rules live. It was the highest-consequence, lowest-coverage code we had.
3. **Security pipeline.** I migrated our CI security scanning from Fortify to Checkmarx across two
   repositories, wiring up both static analysis and software-composition analysis.

**Result.** ⚠️ **YOU MUST FILL:** any measured outcome — build time, scan findings, latency. If you
never measured it, say "we didn't instrument it, so I can only speak to the structural change." That
honesty reads as senior.

**The transferable point:** "Deleting the synchronous overload is the actual fix. Adding the async one
is just an option."

---

## Story 5 — "Tell me about a time you had to learn a domain fast"

**Use for:** adaptability, culture-fit, career-change framing.
**Based on:** the regulatory interceptor work.

**Situation.** I joined a platform where the hard part was not the code — it was that the same
endpoint had to behave differently in 15+ jurisdictions, each with its own regulator. Pennsylvania KYC
is not Portugal KYC. Brazil self-exclusion is not UK self-exclusion.

**Task.** Build and modify compliance interceptors correctly, where "correctly" is defined by a
regulator's rulebook, not by a spec I could reason my way to.

**Action.** I learned to treat the requirement as the source of truth and verify rather than infer —
reading the actual regulatory requirement in the story, checking the live contract or WSDL of the
downstream service rather than assuming its shape, and asking the market-side stakeholder when the
ticket was ambiguous. Over time I shipped interceptors for Pennsylvania, Portugal, Spain, Italy, the
UK, Brazil, Alberta and Ontario, plus the Responsible Gaming surface — deposit limits, session limits,
stake limits, self-exclusion reversal, end-of-year statements.

**Result.** ⚠️ **YOU MUST FILL:** an example where you caught a wrong assumption *before* it shipped.
You will have one. That is the real payload of this story.

**The transferable point:** "In a regulated domain, 'it compiles and the tests pass' is not done. The
question is whether it matches the rule."

---

## Story 6 — "What are you working on / do you code outside work?"

**Use for:** the closing question, enthusiasm signal.
**Based on:** the MCP server and the Angular track — both real, both yours.

"Two things. I'm building an MCP server in .NET that resolves C# type definitions out of a real
solution using Roslyn's `MSBuildWorkspace`, so it finds first-party source types and not just compiled
metadata — and packaging it as a .NET global tool so any MCP client can use it with zero config. The
interesting constraint is that stdio is the transport, so anything written to stdout corrupts the
JSON-RPC stream; all logging has to go to stderr. And I'm deliberately designing it so the tool never
throws — every failure comes back as a structured result with workspace diagnostics attached, because
'no match found' and 'your solution failed to load' are very different answers.

Alongside that I'm working through Angular properly — not tutorial-following, but understanding
change detection, signals and RxJS operator choice well enough to defend the decisions — because I
want to be able to own a feature front to back rather than stopping at the API boundary."

⚠️ **YOU MUST FILL:** have the repo link ready and the thing actually running. This story is only
strong if the code exists. See the 30-day plan.

---

## Three questions you should ask them

Asking good questions is the cheapest seniority signal available.

1. "How do you handle a change that has to behave differently per market or per customer tier — is
   that configuration, feature flags, or branching logic in the code?" *(You have real opinions here.)*
2. "What does your release process look like, and who can ship to production on a Friday?"
3. "What's the oldest part of this codebase, and what's the plan for it?"

---

## On the thing you asked me about

You asked me to help you bluff. I'm not going to, and here is the practical reason rather than the
moral one.

Interviews for engineers are not résumé checks, they are depth probes. An interviewer picks a bullet
and asks "why did you do it that way?" then "what else did you consider?" then "what broke?" A real
story goes five levels deep because you were there. An invented one dies at level two, and the
interviewer's conclusion is not "weak answer" — it is "this person will lie to me about a deadline."
That ends candidacies and it travels through referral networks.

You do not need it. You shipped 81 merged changes to a platform that handles real money under
regulatory scrutiny in 15+ countries, you cut 10 releases, you were the reviewer other engineers
waited on, and other teams escalated to you by name. Most candidates with your years of experience
cannot say any of that. The work is done. What was missing was the inventory — and that is now in
`EVIDENCE-PACK.md`.

Go and tell the truth confidently. It is a much stronger hand than the one you were reaching for.
