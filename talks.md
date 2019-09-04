# Talks

This document contains notes that I have taken while watching conference talks.

## [Pilot Decision Management (Clifford Agius)](https://www.youtube.com/watch?v=QNA9EExd8lQ)

The TDODAR framework:

### Time

- Is it an emergency? Do we have to act quickly?
- Do we need to and can we make more time?
- Do we have time for a cup of coffee?
- Start a stopwatch and make sure you come back to the "T" to check if things
  change

### Diagnosis

What do we think happened?

- Discuss the symptons
- Ask open questions
- Find some information to tell me this is not XYZ
- Agree on the issue to be tackled
- Make it quick and concise, the clock is ticking!

### Options

What should we do?

- Brainstorm possible options
- Tell people "Give me another option!" if the discussion dries up
- Take input from all members of the team and outside sources
- No such thing as a silly idea. Verbalize everything
- Don't drag it out, be quick. Often the first ideas are the best anyway

### Decide - What are we going to do?

- As a team decide what is the correct or chosen path
- Don't spend too much time deciding, pick an option and go with it
- **State the decision**

### Assign

Assign tasks to the members of the team

- Team leader assigns tasks
- Make tasks short and within the skills of that team member
- It is not a race!
- Complete your task as well as you can but don't delay completion
- If you can't complete ask for help
- Consider overload

### Review

- Has the issue been resolved?
- Do we still have time?
- Quickly repeat TDODAR to see if actions have changed the answer
- Is it still a good decision?

## [Sharpening the Tools (Dan North)](https://www.infoq.com/presentations/Sharpening-the-Tools)

Become a Software Craftsman:

### Novice Programmer

Need rules (**not patterns**) to guide their way: Don't ask. Follow this advice
and you will be fine.

### Advanced Beginner

Do not follow the rules! Find out why the rules are the rules. We are starting
to get context - we experience how stuff works.

### Competent

You become goal oriented. This is a time based thing. Most people become
competent if they keep doing something.

### Proficient

This is a deliberate step. Things start to become intuition. Patterns start to
become useful. "How can I make this better?"

### Expert

You are operating of instinct. You don't think about rules, you "just know".
This is critical: You don't know how you come up with your decision.

## [Learn to love meetings (Dr. Neil Roodyn)](https://www.youtube.com/watch?v=ppfLUFO-hwc)

- Have a timeline and an agenda
- "Check-in": Say your name, how you feel and your expectations at the beginning
  of a meeting
- Decisions are made via votes
  - Yes
  - I don't care
  - No (you have to provide an alternative to discuss and vote)
- A decision can be postponed through an "investigation". This is used to ask
  clarifying questions
- They were using a dashboard to display metrics to analyze how the meeting time
  was spent (e.g. fiddling with the projector, actual discussions and so on)
- Lean Coffee


## [Preventing the Collapse of Civilization (Jonathan Blow)](https://www.youtube.com/watch?v=pW-SOdj4Kkk)

- Technology on its own will degrade. It needs constant effort to improve and
  not lose technology
- Without generational transfer, civilizations die
- How productive are programming languages at a higher abstraction level (think
  C#, Haskell or JavaScript) **really**? The first version of Unix was written
  in three weeks
- We keep adding complexity, which means that each individual knows less and
  less about a system
- We are reducing development time by using existing tools and frameworks, but
  we are also giving up capability. This is fine in isolation, but it might
  become a problem if everyone does it
- Only a handful of people **really** know how a CPU works
- Our tools change our thought process

## [Don't Walk Away from Complexity, Run (Venkat Subramaniam)](https://www.youtube.com/watch?v=4MEKu2TcEHM)

- Two kinds of code frustrate him:
  - One that won't work
  - One that works, but shouldn't
- Shared mutability is the devil's work
- Using a library is like dating, using a framework is like getting married

## [Green is Good Red is Bad - Turning your Checklists into Pester Tests (Rob Sewell)](https://www.youtube.com/watch?v=Qy-uvT57pt8)

- You can use Pester (a PowerShell unit testing framework) to test your
  infrastructure
- Use the NunitXML output format together with `reportviewer.exe` to create
  pretty HTML reports. Management loves them

## [Transactions - Myths, Suprises and Opportunities (Martin Kleppmann)](https://www.youtube.com/watch?v=5ZjhNTM8XU8)

ACID is more or less a marketing term, it isn't too precise.

**Durability:**

Used to mean that your database is written to an archive tape. When tape bands
fell out of fashion, durability was redefined as "fsync to disk". With the rise
of distributed system, durability was redefined once more to mean replication.

**Consistency:**

- This is not the same C as in the CAP theorem
- A database moves from one consistent state to the another through
  transactions. A consistent state is defined through integrity checks or
  invariants (e.g. the balance of an account cannot be negative)
- It is a property of how the application uses the database, it is not a
  property of the database itself

**Atomicity:**

- "All or nothing guarantee"
- It's about handling crashes/fault, not about concurrency! You either get all
  or no parts of a transaction

**Isolation:**

Serializable isolation means that the effects of concurrent transactions is as
though all transactions were performed in a serial (one after the other)
fashion. Each transaction feels as if it had the whole database for itself.

Databases have different default and maximum isolation levels. These levels are:

- Read Uncommitted
- Read Committed:
  - Dirty reads/writes are not allowed
  - Does not prevent Read Skew (see below). This is scary, as "Read Committed"
    is the default isolation level for several databases
- Snapshot Isolation:
  - Read skews are not allowed. If a transaction is reading the database, the
    transaction sees the database at a specific point in time. Other
    transactions do not interfere.
  - Does not prevent Write Skew
- Repeatable Read
- Serializable
  - Can prevent Write Skew
  - Some implementations use two-phase-locking (not two-phase-commit!), which
    use shared locks. This can be problematic, as analytical queries lock the
    whole databases.
  - Other solutions (which don't use shared locks) are H-Store and Serializable
    Snapshot Isolation

**Race conditions:**

- Dirty Read: A transaction cannot read what another unfinished transaction
  wrote
- Dirty Write: Concurrent writes to several tables can interfere with each other
- Read Skew: Imagine a transaction which transfers 100 dollars from one account
  to another. A backup process might read both accounts at different times (one
  before a transaction, and one afterwards), which means that the backup now
  contains inconsistent data
- Write Skew:
  - Pattern: Read something, make decision, write decision to database
  - Example: An ambulance system requires, that each shift has at least one
    doctor on call. If several doctors request to go off call at the same time,
    we can end up in a situation in which no doctor is on call. This can happen
    because these concurrent transactions see the exact same snapshot of a
    database
  - "By the time the write is committed, the premise of the decision is no
    longer true"

## [How did we end up here (Todd Montgomery & Martin Thompson)](https://www.youtube.com/watch?v=oxjT7veKi9c)

- Focus on the fundamentals. Master them and understand them before you try
  to change them
- Shared mutable state is **a complete nightmare** and should only be used for
  systems programming. The smartest people get this wrong all the time
  - A cache is one the hardest problems in computer science. Do you **really**
    want to implement it yourself?
  - Embrace append-only, single writer, and shared nothing designs
- Universal scalability law: You can't run away from math
- Stop using text encoding. The web is in a constant "debug mode"
- Synchronous communication is the crystal meth of distributed programming.
  Remote Procedure Calls do not work
- Object orientation and set theories are great models. Please don't use ORMs to
  make them work together. If you don't understand SQL, please do not use a
  database
- "The purpose of abstraction is not to be vague, but to create a new semantic
  level in which one can absolutely precise" - Dijkstra
- Think in terms of transformation and flow of data - not code!
- Farley's second law: "As soon as you realize that most people don't know what
  they are doing the world makes a lot more sense.."

## [It's about time (Christin Gorman)](https://www.youtube.com/watch?v=Nhhm5yC2HCo)

The basic time library in your favorite programming language might be horrible.
Why? Because they tend to mix two very different concepts:

- The linear progression of time
- An interpretation of time, based on politics, astronomy and history

What time is it? 1532428776
No, I mean what time is it? Well, that depends. Which epoch do you mean?

| Environment | Start      |
|-------------|------------|
| .NET        | 1 Jan 0001 |
| Windows     | 1 Jan 1601 |
| Uni         | 1 Jan 1970 |
| GPS         | 5 Jan 1980 |

A timestamp on Windows means something completely different than a timestamp on
Unix!

Time synchronization (clock drift correction) is the reason why Windows does not
guarantee, that the system time increases monotonically. So you shouldn't use
it. Instead, use something different like the current tick count, or use your
own sequence number.

UTC (which stands for Coordinated Universal Time. Yes, UTC. Yep.) is an effort
to create a system on which we can all agree.

Coding advice:

- Store timestamps as UTC together with a time zone
- Do not store start/end timestamps. Instead, store a start timestamp together
  with a duration. This makes it much easier to deal with events such as
  day-light saving
- Don't always mock our your database layer. The conversation of dates (which
  can depend on the time zone of your database **and** on the time zone of your
  operating system) will hunt you down
- Make date ranges **inclusive** from and **exclusive** to (start <= value <
  end)

## [PID Loops and the Art of Keeping Systems Stable (Colm MacCárthaigh)](https://www.youtube.com/watch?v=3AxSwCC7I4s)

Control theory:

Present -> Observe -> Feedback -> React -> (Present)

A furnace is a classical example of applied control theory: you want to keep
water at a specific temperature. So what do you do? You measure the error (e.g.
the water has 20°C, it should be 100°C, so the error is 80°C) and react with
correcting actions based on the error. To do this, we distinguish three types of
controllers:

### P Controller

- Takes proportional steps to correct an error (e.g. the applied heat is
  proportional to the measured error)
- These systems tend to oscillate around the desired state

### PI Controller

- Adds an integral to observe an error over time
- Such a system still oscillates, but the overall error curve is flattened
- Thermostats or cruise controls use PI systems
- These systems cannot deal with shocks

### PID Controller

- Adds a derivative component

### Anti Patterns

Using open loops is scary. The system cannot detect a problem. Chaos engineering
and observability are fine practices to find open loops. Open loop systems tend
to be imperative (do this, do that), while closed loop system tend to be
declarative (please get the system into my defined desired state).

Power laws are out to get you. A system failure can spread in an exponential
way. These failures can be kept in their cages by building smaller systems
(which decrease the overall "blasting radius"). Other techniques include:

- Exponential back-off
- Rate-limiters

Sudden load spikes can bring down a system. In general: keep your queues short.
LIFO queues might be a good idea, as they will prioritize new information.

Implementing edge triggered systems imply, that you have solved the "deliver
just once" problem. Level triggered (and idempotent) systems seem to be a
simpler solution.
