# The Tao of Coaching (Max Landsberg)

|               | **Low Skill** | **High Skill** |
| ------------- | ------------- | -------------- |
| **High Will** | Guide         | Delegate       |
| **Low Will**  | Direct        | Excite         |

# CTO

I can't remember where I picked up this quote:

> As a CTO, my default loop is "First, cycle through all my employees and make
> sure that I have equipped them to be happy and productive in their jobs.
> Second, find something to do. If possible, delegate it; if not, do it.
> Repeat."

# Decision Logs

Create logs that hold the following information:

- Problem you are deciding on
- Involved people
- What did you choose? Why? Risks?
- What other things have you considered? Why didn't you choose them?

Michael Nygard described his decision log structure in [this blog
post](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions):

- **Context:** Describe the forces at play
- **Decision:** Your response to these forces
- **Status:** The decision status (e.g. "proposed")
- **Consequences:** What happened afterwards?

# Backups

Backups don't mean anything unless you have actually tried to restore data from
them.

# Modularity

"Start with a list of difficult design decisions or design decisions which are
**likely to change**. Each module is then designed to **hide such a decision**
from the others." - David Parnas

The goal of modularity is not to make things easier but to hide the things that
are hard.

# How To Deal With Configuration

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

# SBI Model

- **Situation:** In the team meeting on Friday
- **Behavior:** you spoke across me several times
- **Impact:** so I felt like I wasn't being allowed to share my opinion with the
  team

# Cynefin Framework

- **Chaotic:** A crisis. Your best bet is to stabilize the situation
- **Complex:** (Creativity) You don't know what you don't know, so you have to try things out
  to understand your environment
  - Make the right system
- **Complicated:** (Skill) You kinda know what's going on so you can engineer a solution
  - Make the system right
- **Obvious:** (Automation) You can automate these steps

# [Four Kinds of Documentation](https://www.divio.com/blog/documentation)

- **Tutorial:** learning-oriented; a getting started guide
- **How-To Guide:** goal-oriented; how to solve a specific problem
- **Explanation:** understanding-oriented; provides background and context
- **Reference:** information-oriented; describes inner processes

# Limiting Beliefs

- Time = Money
- Technical excellence is everything

# A Good Code Base

The user "l0b0" outlined on [Hacker
News](https://news.ycombinator.com/item?id=24611256) what he expects in a good
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

# Design Principles

J. B. Rainsberger mentioned this in his talk [Programming Is The Easy
Part](https://www.youtube.com/watch?v=SbGiSH_8UGk):

A lot of high level software design principles boil down to a linear combination
of "remove duplication" and "improve names".

# [ELI5 Concurrent And Parallel Programming](https://joearms.github.io/published/2013-04-05-concurrent-and-parallel-programming.html)

Created by Joe Armstrong:

![Coffee Example](./con_and_par.jpg)

# Hiring Process

- [Resource](https://thetechresume.com/) to help a developer in the hiring
  process

# CLI Guideline

- [Written guide](https://clig.dev/)

# Cryptography

- [Online book](https://cryptobook.nakov.com/)

# Chaos Engineering

Adding parameters for sleep timers and error rates to service requests can be an
easy way to test how a specific service deals with issues in isolation.

# Database Migrations

Based on [this article](https://martinfowler.com/articles/evodb.html):

- Every developer has his/her own database. The build script offers ways to
  create and delete a database based on a configuration file (which is not put
  into version control). This configuration file contains the database name as
  well as a connection string
- Migration scripts (and test data) are put into version control

# The Phoenix/Unicorn Project (Gene Kim)

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

# CAP Theorem

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

Martin Kleppmann posted an [interesting article][klepp] in which he explains
that neither CAP nor PACELC are a good way to think about distributed systems.

# Idealcast

Taken from The Idealcast podcast episode 14:

Employee satisfaction can indicate the performance of an organization. Ask:

- Are you happy?
- Are you able to do work that you are proud of?

[klepp]: https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html

# Believes

- Be prepared to throw away something you’ve done in order to do something
  different.
- Always look for better ways of doing things.
- “Good enough” isn’t good enough.
- Code is a liability, not an asset
- Treat code like a garden. You are never "done"
- Prefer immutability
- Model actions as data
- Abstractions and data schemas are hard to get right. You will most likely
  change them in the future, so prepare yourself
