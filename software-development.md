# Software Development

## Speakers

- Kevlin Henney
- Rich Hickey
- Greg Young
- Michael Feathers
- Dan North
- Martin Fowler
- Martin Thompson
- Todd Montgomery
- Bryan Cantrill
- Mark Seemann
- Jimmy Bogard
- Jonathan Blow
- Simon Brown
- Sam Newman
- J. B. Rainsberger
- Chad Fowler
- Bret Victor
- Martin Kleppmann
- Kent Beck
- Linda Rising
- Casey Rosenthal
- Stefan Tilkov
- Michael Nygard
- Allen Holub
- Jez Humble
- John Hughes
- Rafal Dittwald
- Michael Bryzek

## CTO

I can't remember where I picked up this quote:

> As a CTO, my default loop is "First, cycle through all my employees and make
> sure that I have equipped them to be happy and productive in their jobs.
> Second, find something to do. If possible, delegate it; if not, do it.
> Repeat."

## Decision Logs

Create logs that hold the following information:

- Problem you are deciding on
- Involved people
- What did you choose? Why? Risks?
- What other things have you considered? Why didn't you choose them?

Michael Nygard described his decision log structure in [this blog
post][architecture-decisions]:

- **Context:** Describe the forces at play
- **Decision:** Your response to these forces
- **Status:** The decision status (e.g. "proposed")
- **Consequences:** What happened afterwards?

[architecture-decisions]: https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions

## Modularity

"Start with a list of difficult design decisions or design decisions which are
**likely to change**. Each module is then designed to **hide such a decision**
from the others." - David Parnas

The goal of modularity is not to make things easier but to hide the things that
are hard.

## How To Deal With Configuration

- Rely on useful default configuration which is stored inside the application:
  - e.g. using a log folder next to the application folder, using default
    schedule timers, etc.
  - Maybe it would make sense to create a default configuration file if it does
    not exist
- Allow external configuration via several sources. They can override each
  other:
  - Default values stored in the application
  - Configuration files
  - Command line arguments
  - Environment variables
- Throw a meaningful error on configuration problems
- Try to initialize resources if they are not found (e.g. create directories,
  create files, ...)
- Create application arguments which allows a user to specify a configuration
  file in a specific place
- Some configuration (e.g. plugging an application into ElasticSearch) should be
  disabled by default. A user can enable them if he wants to

## SBI Model

- **Situation:** In the team meeting on Friday
- **Behavior:** you spoke across me several times
- **Impact:** so I felt like I wasn't being allowed to share my opinion with the
  team

## Cynefin Framework

- **Chaotic:** A crisis. Your best bet is to stabilize the situation
- **Complex:** (Creativity) You don't know what you don't know, so you have to try things out
  to understand your environment
  - Make the right system
- **Complicated:** (Skill) You kinda know what's going on so you can engineer a solution
  - Make the system right
- **Obvious:** (Automation) You can automate these steps

## [Four Kinds of Documentation](https://www.divio.com/blog/documentation)

- **Tutorial:** learning-oriented; a getting started guide
- **How-To Guide:** goal-oriented; how to solve a specific problem
- **Explanation:** understanding-oriented; provides background and context
- **Reference:** information-oriented; describes inner processes

## A Good Code Base

The user "l0b0" outlined on [Hacker News][codebase] what he expects in a good
code base:

> - A working CI pipeline.
> - Comprehensive tests.
> - Enforced linting/formatting.
> - A script (or at least documentation) to set up a dev env from scratch
>   (including resetting it if it already exists).
> - Documentation of things which would take days or weeks to discover when
>   working without the original developers, such as high-level trade-offs, code
>   style which is not enforceable using linting (such as "Use Foo.frobnicate
>   rather than the built-in frobnicator because ...") references to issue trackers,
>   former non-committing project-related people such as business analysts,
>   separate documentation, credentials etc.

[codebase]: https://news.ycombinator.com/item?id=24611256

## Chaos Engineering

Adding parameters for sleep timers and error rates to service requests can be an
easy way to test how a specific service deals with issues in isolation.

## Database Migrations

Based on [this article][evodb]:

- Every developer has his/her own database. The build script offers ways to
  create and delete a database based on a configuration file (which is not put
  into version control). This configuration file contains the database name as
  well as a connection string
- Migration scripts (and test data) are put into version control

[evodb]: https://martinfowler.com/articles/evodb.html

## The Phoenix/Unicorn Project (Gene Kim)

Three Ways

- System Thinking
- Amplify Feedback Loops
- Culture Of Continual Experimentation And Learning

Five Types Of Work

- Business Projects
- Internal IT Projects
- Changes generated by the above
- Unplanned Work

Five Ideals

- Locality and Simplicity
- Focus, Flow, and Joy
- Improvement of Daily Work
- Psychological Safety
- Customer Focus

## CAP Theorem

- **C**onsistency
- **A**vailability
- **P**artition tolerance

This theorem states that given a network partition (a split brain situation), a
distributed system can either favor consistency or availability.

**Example:** Imagine that you are part of a fully remote wedding. Let's imagine
that you answer "the big question" with yes and let's imagine that your
phone/internet connection breaks before your significant other can answer the
same question. What do you do? Do you say that you are married/not married (in
which case you would favor availability) or do you say "I don't know" (in which
case you would favor consistency).

The PACELC theorem is an extension to the CAP theorem: In case of a network
**P**artition, one has to choose between **A**vailability and **C**onsistency,
**E**lse, when a system does not have a partition, one has to choose between
**L**atency and **C**onsistency.

Martin Kleppmann posted an [interesting article][klepp-cap] in which he explains
that neither CAP nor PACELC are a good way to think about distributed systems.

[klepp-cap]: https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html

## [Idealcast Podcast](https://itrevolution.com/the-idealcast-podcast/)

Episode 14:

Employee satisfaction can indicate the performance of an organization. Ask:

- Are you happy?
- Are you able to do work that you are proud of?

## Believes

- Working code is only the beginning
- Be prepared to throw away something you’ve done in order to do something
  different
- Always look for better ways of doing things
- “Good enough” isn’t good enough
- Code is a liability, not an asset
- Treat code like a garden. You are never "done"
- Prefer immutability
- Model actions as data
- Abstractions and data schemas are hard to get right. You will most likely
  change them in the future, so prepare yourself
- Time != Money
- Technical excellence is not everything
- Backups don't mean anything unless you have actually tried to restore data
  from them
