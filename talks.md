# Talks

This document contains notes that I have taken while watching conference talks.

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
- Using a library is like dating, using a framework is like entering into a
  marriage

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
  invariantes (e.g. the balance of an account cannot be negative)
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
  - Example: An ambulancy system requires, that each shift has at least one
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
- Synchronous communication is the crystal meth of distriuted programming.
  Remote Procedure Calls do not work
- Object orientation and set theories are great models. Please don't use ORMs to
  make them work together. If you don't understand SQL, please do not use a
  database
- "The purpose of abstraction is not to be vague, but to create a new semantic
  level in which one can absolutely precise" - Dijkstra
- Think in terms of transformation and flow of data - not code!
- Farley's second law: "As soon as you realise that most people don't know what
  they are doing the world makes a lot more sense.."
