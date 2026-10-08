# 30-Day Job Search Plan
**1 Oct 2026 → 31 Oct 2026**

Target: **offer in hand by 31 October.** Realistic, because a .NET backend engineer with ~2 years on a
regulated payments-adjacent platform is a liquid profile in Hyderabad right now.

Honest framing up front: the thing most likely to cost you the month is not skill, it is **pipeline
volume**. Interviews are a conversion funnel. At ~2 YOE expect roughly 100 applications → 12–15
screens → 5–6 onsites → 1–2 offers. So the plan front-loads applications and referrals, and runs
preparation *in parallel* rather than before.

---

## Week 0 — Today and tomorrow (1–2 Oct). Do not skip.

These are time-critical because access and goodwill both decay the moment you're off the payroll.

- [ ] **Export your own record now, while you still have access.** `EVIDENCE-PACK.md` is already
      written — read it once and add anything from memory that the tools couldn't see.
      ⚠️ Facts, dates and your own accomplishments only. Do **not** copy out source code, internal
      documents, customer data, credentials, or architecture docs. That is the line between
      "professional record" and "IP theft," and crossing it can end a career.
- [ ] **Fill the `[BRACKETS]` in `RESUME.md`.** Exact dates and years from your offer letter.
- [ ] **Get your personal email and phone onto everything.** Your work address dies.
- [ ] **Ask for referrals and recommendations today** — templates in §Outreach below. Work through
      your manager, the senior engineers who reviewed your merge requests, the platform leads you
      shipped releases with, and the teammates whose MRs you reviewed. People say yes in week one of
      a layoff. By week four they've moved on.
- [ ] **Clarify your own terms in writing:** last working day, notice/garden leave, severance,
      whether you can interview during notice, and — important in India — whether you'll get a
      **relieving letter and experience letter**. Ask HR for these *before* your last day.
- [ ] **Connect on LinkedIn with everyone you worked with**, before you lose the directory.

---

## The daily rhythm (Mon–Fri, ~7 hrs)

Consistency beats intensity. Protect the morning block.

| Time | Block | What |
|---|---|---|
| 09:00–11:00 | **Applications** | 6–8 tailored applications. Highest-leverage hours — do this before anything else. |
| 11:00–12:00 | **Outreach** | 5 referral asks / recruiter DMs / follow-ups on stale threads. |
| 12:00–13:00 | Break | Actually break. This month is a marathon. |
| 13:00–15:00 | **DSA practice** | 2–3 problems, timed, out loud. |
| 15:00–16:30 | **Deep prep** | Rotating: .NET internals → system design → your own story drills. |
| 16:30–18:00 | **Portfolio** | Ship the MCP server, then the Angular capstone. |
| 18:00–18:30 | **Tracker + tomorrow's list** | Update the tracker, queue tomorrow's 8 applications. |

Saturdays: mock interviews and catch-up. **Sundays off.** Burning out in week three is the common
failure mode.

---

## Week 1 (1–7 Oct) — Foundation + flood the funnel

**Goal: resume final, LinkedIn live, 40 applications out, 15 referral asks sent.**

- **Resume.** Fill in brackets. Export a clean single-column PDF named
  `Bhairi-Harish-Backend-Engineer-DotNet.pdf`. Flatten the markdown tables to plain lines for ATS.
- **LinkedIn.** New headline, About section and Open-to-Work (recruiters-only or public — public gets
  more inbound, and post-layoff there's no stigma). Copy below.
- **Apply.** 8/day. Where, in priority order:
  1. **Referrals** — 5–10× the response rate of cold applications. Always try this route first.
  2. **Product companies hiring .NET in Hyderabad/Bangalore/Pune or remote:** Microsoft, Amazon,
     ServiceNow, Salesforce, Qualcomm, Providence, Optum/UHG, Thoughtworks, Zeta, PhonePe, Navi,
     Groww, Rippling, Icertis, Chargebee, Postman, Hashedin, Sprinklr, Darwinbox, Pixis, Gameberry.
  3. **Other iGaming / betting platforms** — your domain is a direct match and they pay for it:
     Flutter (FanDuel/Paddy Power), Playtech, Sportradar, Dream11, Games24x7, Zupee, MPL, Betsson,
     bet365 (has India presence), SG/Light & Wonder.
  4. **Fintech and regtech** — regulatory compliance engineering transfers almost perfectly.
  5. Instahyre, Cutshort, Wellfound, Hirist, Naukri (recruiters live here; keep the profile fresh —
     a daily login genuinely moves you up search results).
- **Prep.** DSA: arrays, strings, hashmaps. .NET: `async`/`await` and the task scheduler, GC and
  generations, `IDisposable`, value vs reference types, `IEnumerable` vs `IQueryable`.
- **Portfolio.** Resume the MCP server. Follow the `PROGRESS.md` resume block: add the four projects to
  `CSharpTypeFinder.slnx`, wire package references and `Directory.Build.props`, get `ServerInfoTool`
  answering over stdio. **Push it to public GitHub from day one** — a repo with real commit history
  across the month is itself a signal.

## Week 2 (8–14 Oct) — Interviews start landing

**Goal: 40 more applications, 5+ screens completed, MCP server demoable.**

- Expect first recruiter screens. Have the 90-second pitch and your salary number ready (§Negotiation).
- **Prep.** DSA: trees, graphs, two pointers, sliding window. .NET: dependency injection lifetimes,
  middleware pipeline, `IHostedService`, EF Core change tracking and N+1. SQL: indexes, execution
  plans, deadlocks, isolation levels — you have real MSSQL experience, make it concrete.
- **System design** (2 YOE scope — component design, not planet-scale): design a rate limiter, an
  idempotent payment endpoint, a session/token service. You've *built* the third one; lead with it.
- **Portfolio.** MCP server tasks 2–4: validation, result model, Roslyn workspace provider, type
  search. Write the README with client registration instructions. Ship something runnable.

## Week 3 (15–21 Oct) — Convert

**Goal: 30 more applications, 3+ onsites, first offers appearing.**

- Reallocate: as interviews fill the calendar, drop to 4 applications/day and spend the rest on
  company-specific prep. A scheduled onsite is worth more than ten new applications.
- **Debrief every interview the same day** — questions asked, what you fumbled, what you'd say next
  time. This compounds faster than anything else you can do.
- **Prep.** DSA: DP basics, heaps, intervals. Behavioural: run all six stories from
  `INTERVIEW-STORIES.md` out loud, once each, with the `⚠️ FILL` gaps closed.
- **Portfolio.** MCP server tasks 5–7. Then `dotnet pack` and publish the global tool. Tweet/post it —
  a shipped, installable tool is a stronger artifact than a half-built web app.

## Week 4 (22–31 Oct) — Close

**Goal: offer accepted, or 2+ in final rounds with a clear timeline.**

- Chase every stalled thread: "Following up on the `[role]` — I'm in final stages elsewhere and would
  love to keep this moving." Honest urgency works; manufactured urgency backfires if they check.
- **Negotiate.** Never accept on the call. "Thank you — can you send the written breakdown? I'll
  respond within two days." Then negotiate base first, joining bonus second.
- If no offer by the 28th: that's not failure, it's data. The fix is almost always volume or a specific
  stage you keep losing. Check the tracker for the pattern.
- Keep the Angular capstone running at low intensity — it's your Q1 story, not an October blocker.

---

## LinkedIn copy

**Headline**
```
Backend Engineer | .NET, C#, ASP.NET Core, Distributed APIs | Ex-Entain | Open to opportunities
```

**About**
```
Backend engineer specialising in .NET and C#, with experience building and operating the
player-account, authentication and regulatory-compliance API platform behind global betting brands
at Entain.

What I've done: shipped 80+ changes to a high-traffic ASP.NET Core platform serving 15+ regulated
markets across Europe, North America and Brazil. Built passkey/WebAuthn authentication flows and
versioned session-token handling. Delivered compliance interceptors for jurisdictions from
Pennsylvania to Portugal to Brazil, where a defect blocks real-money play for an entire market.
Owned 10 end-to-end production releases, including hotfixes under change freeze. Handled live
incidents up to P2, and reviewed around 70 merge requests from teammates.

What I'm good at: legacy modernisation (SOAP to REST, sync to async), concurrency correctness,
API contract design, and shipping carefully in environments where mistakes are expensive.

Currently building an open-source MCP server in .NET that resolves C# type definitions through
Roslyn, and deepening my Angular to own features end to end.

Open to Backend / Software Engineer roles — Hyderabad, Bangalore, or remote.
Reach me at [personal email].
```

**A post worth making (day 2 or 3).** Post-layoff posts get real reach and recruiters watch them.
```
After [X] years at Entain, my role was affected by the recent restructuring and I'm now looking
for my next backend engineering role.

What I spent that time doing: building the account, authentication and compliance APIs behind
brands like bwin, Ladbrokes and BetMGM — passkey authentication, Responsible Gaming features,
and regulatory logic for 15+ markets. I shipped 80+ changes to that platform, cut 10 production
releases, and spent a lot of nights on P2 incidents where real money was moving.

I'm looking for .NET / C# backend roles in Hyderabad, Bangalore or remote. If you know of
something, or you're hiring, I'd really appreciate a pointer — and I'm happy to return the
favour for anyone else job hunting right now.

#opentowork #dotnet #csharp #backend #hiring
```

---

## Outreach templates

**Referral ask — former colleague at a target company**
```
Hi [Name] — hope you're well. My role at Entain was affected by the recent restructuring, so
I'm looking for backend roles now.

I saw [Company] has a [role] opening ([link]). Would you be open to referring me, or pointing
me to the right person? I've attached my CV — the short version is ~[X] years on .NET /
ASP.NET Core, building the account and compliance APIs behind Entain's brands across 15+
regulated markets.

Completely fine if it's not a good moment. Thanks either way.
```

**Recommendation ask — a lead or reviewer who knows your work**
```
Hi [Name] — my role was affected by the restructuring, so I'm starting a job search.

Would you be willing to write me a short LinkedIn recommendation? If it helps, things you'd
have visibility on: the passkey registration work, the [jurisdiction] compliance interceptors,
the releases I cut, and the MRs I reviewed for the team.

No problem at all if you'd rather not, or if you'd prefer to just be a reference instead.
Thanks for everything — I learned a lot working with you.
```
*Make it easy to say yes. Offering them the specifics means they can write it in two minutes.*

**Cold recruiter / hiring manager DM**
```
Hi [Name] — I'm a backend engineer with ~[X] years in .NET/C#, most recently at Entain building
the authentication and regulatory-compliance APIs behind their betting brands (passkeys,
multi-jurisdiction compliance, 10 production releases, P2 incident response).

My role was affected by a restructuring, so I'm actively looking. I noticed [specific thing
about their team/stack]. Would a 15-minute call be useful?

CV: [link]
```

**How to answer "why did you leave?"** — once, plainly, then move on:
> "Entain restructured and my role was eliminated along with a number of others on the team. It
> wasn't performance-related — happy to connect you with my manager as a reference."

Don't apologise for it, don't over-explain it, don't badmouth Entain. Layoffs are completely normal in
2026 and interviewers know it. The only thing that makes it look bad is you treating it as shameful.

---

## Negotiation notes

- **Know your number before the first screen.** Research your band on Levels.fyi, AmbitionBox and
  Glassdoor for ~2 YOE .NET in Hyderabad. Decide: target, and walk-away.
- When asked current CTC: answering honestly is standard practice in India and refusing can stall the
  process — but always pair it with your expectation. *"My current fixed is ₹X. Based on the market for
  this scope I'm looking at ₹Y, and I'm flexible on structure."*
- **Your domain is a premium.** Regulated gaming/payments platform experience, passkey/WebAuthn, and
  genuine production incident ownership at 2 YOE is not common. Price accordingly.
- Never accept on the phone. Get it in writing. Negotiate once, firmly, politely.
- Being laid off does **not** mean taking the first number. Desperation is the only thing that turns a
  layoff into a pay cut — and with your record you have no reason to be desperate.

---

## Tracker

Make a spreadsheet today. Columns:
`Date applied | Company | Role | Channel (referral/portal/recruiter) | Referrer | Status | Last contact | Next action | Notes`

Review it every Friday and ask two questions:
1. **Volume** — did I hit the application target? If not, that's the whole problem.
2. **Stage** — where am I losing? Resume screen → the resume or targeting is off. DSA round → shift
   prep hours to DSA. Behavioural → drill the story pack. System design → focus there.

Fix one thing per week, not five.

---

## The one thing to remember

You came into this asking for help inventing a track record. You already have one: **87 merge
requests, 81 merged to production, ~70 code reviews for your teammates, 10 releases shipped, 7
production incidents including a P2 across eight countries, and compliance code live in 15+ regulated
markets — at an SDE I level.**

The only real gap was that nobody had written it down. Now it is written down, in `EVIDENCE-PACK.md`,
with the ticket numbers to back every line.

Go sell the truth. It's better than the story you were going to make up.
