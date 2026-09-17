# JASSAS Data Engineering Take-Home Assessment

Thanks for making it to this stage. This is a practical, scoped take-home meant to
reflect the kind of problem we actually work on, not a trivia quiz.

## Context

JASSAS is a Saudi corporate-intelligence platform. A core part of the product is
figuring out **who owns whom** — ownership structures, corporate relationships,
and ultimately, who the real (natural-person) beneficial owner of a company is.
That's the problem this assessment is built around, using a live public data
source instead of a static file, since real ingestion work always means dealing
with a data source that can be slow, inconsistent, or partially wrong.

## The task

Build a small pipeline that, for each company in [`seed_companies.md`](seed_companies.md):

1. **Resolve** the company name to the correct entity on
   [Wikidata](https://www.wikidata.org), using Wikidata's public API
   (`wbsearchentities` / `wbgetentities`) or the SPARQL endpoint
   (`query.wikidata.org/sparql`). Some of these names are ambiguous or collide
   with unrelated entities — you'll need to decide how to pick the right one,
   and say how in your writeup.
2. **Pull** each resolved entity's ownership-relevant data live from the API:
   things like "owned by", "owner of", "subsidiary", "parent organization", and
   key people (CEO, chairperson) where present. Handle pagination/batching and
   be resilient to rate limits or transient errors — this is a live API, not a
   fixture, and it will occasionally be slow or flaky.
3. **Land and model** the data: keep a raw landing layer separate from your
   normalized/modeled layer(s). Use whatever layering convention you like
   (bronze/silver/gold, staging/marts, etc.) as long as raw vs. modeled is
   clearly separated.
4. **Answer**, for each seed company: *who ultimately owns/controls this
   company?* Follow ownership chains where they exist (company A owned by
   company B owned by person C), and explicitly flag any case that's
   unresolved, ambiguous, missing data, or circular — don't silently drop or
   guess at these.

## Constraints

- Hit the **live** Wikidata API/SPARQL endpoint at runtime. Don't bundle a
  static snapshot of the data as a shortcut.
- Load your modeled data into a real database. Postgres is preferred (a
  starter [`docker-compose.yml`](docker-compose.yml) is included); SQLite is
  acceptable if that's easier for you to run locally.
- Use whatever language/framework you're comfortable with. Python is our
  team's primary language so it's the easiest for us to review, but it's not
  required.
- Your ingestion step should be **safely re-runnable** — running it twice
  shouldn't duplicate rows or corrupt state.

## What to hand back

- Your code, in your own fork (see **Submission** below).
- A filled-in [`SOLUTION.md`](SOLUTION.md) — a template with the sections we
  care about is already in this repo. Use it as a starting point; reshape it
  if a different structure fits your solution better, but keep the substance.

There's no fixed format for the code itself — structure it however you'd
structure a real project.

## Time

We expect this takes roughly **4-8 focused hours**. You'll have about a week
from receiving this to send it back, but that's scheduling slack around your
own time, not an expectation you'll spend the whole week on it.

## Submission

1. Fork this repository to your own GitHub account.
2. Do your work in your fork, on any branch you like.
3. When you're ready, send Mahmoud the link to **your fork** directly.
   Please don't open a pull request against this template repository — we'd
   rather other candidates not be able to see your solution (or you, theirs).

Questions welcome — reach out rather than guessing at something ambiguous in
these instructions (though ambiguity in the *data itself* is intentional and
part of the exercise).
