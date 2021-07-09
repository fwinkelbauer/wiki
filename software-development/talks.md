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

- Discuss the symptoms
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

- Dirty Read: A transaction can read what another unfinished transaction wrote
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

What time is it? 1532428776. No, I mean what time is it? Well, that depends.
Which epoch do you mean?

| Environment | Start      |
|-------------|------------|
| .NET        | 1 Jan 0001 |
| Windows     | 1 Jan 1601 |
| Unix        | 1 Jan 1970 |
| GPS         | 5 Jan 1980 |

A timestamp on Windows means something completely different than a timestamp on
Unix!

Time synchronization (clock drift correction) is the reason why Windows does not
guarantee, that the system time increases monotonically. So you shouldn't use
it. Instead, use something different like the current tick count, or use your
own sequence number.

UTC (which stands for Coordinated Universal Time) is an effort to create a
system on which we can all agree.

Coding advice:

- Store timestamps as UTC together with a time zone
- Do not store start/end timestamps. Instead, store a start timestamp together
  with a duration. This makes it much easier to deal with events such as
  day-light saving
- Don't always mock out your database layer. The conversation of dates (which
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

- Adds a derivative component to predict future errors

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

## [Big Numbers and the 1Hz CPU (Tom Hudson)](https://www.youtube.com/watch?v=pDBOC6I3K8g)

We do not have a good intuition for how fast different parts of a computer are.

Let's have a look at a 3ghz CPU and different access times:

- Register: 0.3ns
- L1 cache: 1.5ns
- L2 cache: 3ns
- L3 cache 13ns
- RAM: 0.1 microseconds
- HDD: 6ms
- SSD: 80 microseconds

All these values seem "low enough", but let's but them into perspective using a
1 Hertz CPU:

- Register: 1 second
- L1 cache: 4.5 seconds
- L2 cache: 9 seconds
- L3 cache: 39 seconds
- RAM: 5 minutes
- HDD: 9 months
- SSD: 1 day

## [Design, Composition, and Performance (Rich Hickey)](https://www.youtube.com/watch?v=MCZ3YgeEUPg)

- Design is taking things apart so you can put them back together
- An instrument is a tool for an expert
- You learn an instrument by playing the actual instrument. There is no real
  alternative. This means, that you are using an experts tool while being a
  novice. But you won't be a novice for long
- An instrument is (for the most part) very simple. It is made to work in a very
  specific way. Composers can use several instruments to create a predictable
  outcome. This would be hard if instruments weren't that limited
- A musician spends most of his time practicing instead of performing. Why is
  our industry different?
- We should build interfaces for machines first and then put an interface for a
  human on top
- Constraint is a driver for creativity
- Design is making decisions. It's about saying no

## [Thinking Fast and Slow (Linda Rising)](https://www.youtube.com/watch?v=XjbTLIqnq-o)

**System 1:**

- Unconscious (runs 24/7)
- Fast, intuitive
- Can multi-task
- ~11 million bits/second
- 95% of cognitive function
- inaccessible

**System 2:**

- Conscious
- Slow, rational, forgetful
- Linear (Cannot multi-task)
- ~40 bits/second
- 5% of cognitive function

We identify with System 2 and we believe, that System 2 is in charge.

System 1 gains its speed by using heuristics. It is also in charge of "telling
our story" in which we are identified as the hero. System 1 is prone to biases
such as:

- **Confirmation bias:** We seek confirmation instead of information. We like to
  stick to our point of view, even in the face of evidence which supports a
  different point
- **Cognitive dissonance:** We have a hard time to keep two contradicting ideas
  in our head
- **Naive realism:** We believe that we are rational and that a disagreeing part
  will "see" if we present them "our facts"

We overestimate our own understanding and underestimate the role of randomness
in our world. We seek for patterns and explanations, even if there aren't any.

System 2 can only focus for about 50 minutes (max) before taking a break.

We use System 2 to learn something new. Over time, a certain skill moves to
System 1 (e.g. walking, driving, or playing an instrument). After is has moved
to System 1, interference from System 2 can hurt our performance by
"overthinking".

System 2 takes a lot of energy. Self control causes a drop in your blood
glucose. We have a limited pool of "mental energy". This is why we tend to make
worse decisions when tired or hungry.

System 2 believes that it runs the show, but System 1 is in charge! And that's
good. You don't want to trust a system which lets you forget your keys to care
about essential tasks such as breathing.

**Better Meetings:**

- Water, tea, coffee available
- Standing should be OK
- Very small groups
- Limit meeting times to ~40 minutes. For longer meetings, take a different seat
  after a break
- 10 minute break before important decisions

## [Mistakes and Discoveries While Cultivating Ownership (Aaron Blohowiak)](https://www.youtube.com/watch?v=ddOGmao_cnA)

Netflix Culture:

- **Avoid rules:** Do not constraint people. We need good judgment
- **People over Process:** The world is changing, while your process is lacking
  behind
- **Context not Control:** You can't really good decisions if you do not
  understand your environment. A manager knows less than the "people in the
  field"
- **Freedom and Responsibility:** Have options and hold people responsible for
  the quality of their decision making

Levels of Ownership:

0. **Demonstration:** No ownership
1. **Oversight:** You do it, but we will pre-approve it
2. **Observation:** You do it and we will review it after it is done
3. **Execution:** Here's where we want to go and we know that you will pull it
   off. We might check just so that we know what's going on
4. **Vision:** You understand your responsibilities and your shareholder's needs

Mistakes:

- Different ideas about which level we should be at
- Not being explicit when levels change

## [Changing your Habits & Environment to get more Professional Productivity (Linda Rising)](https://www.youtube.com/watch?v=mrHjHdyRDNY)

- We sit too much and move too little
- Lying down can improve your problem solving skills
- Try to have meetings while walking

## [Functional data that adapts to change (Don Syme)](https://www.youtube.com/watch?v=us4dp7Ksly0)

- Classic UIs are built using the MVVM pattern
- A different approach to building UI is called MVU: Model, View, Update
  - Examples: Svelte, Elm, React Native
- MVU is based on functional principles
- There is a unidirectional data flow
- "UI becomes calculation and information, not state"
- We create a view based on a model and update the model through messages, which
  in turn changes the view
- An initial reaction might be that "functional" and "high performance" cannot
  go together. The key to making it work is "incremental functional
  programming", which is related to event sourcing

## [A Cheap Effective Method for Dealing with Stressful Situations (Linda Rising)](https://www.youtube.com/watch?v=ODpq_6qcPIA)

- The pandemic has created a very stressful environment
- Long periods of anxiety compromises our immune system
- What doesn't work:
  - Suppressing/denying a stressful situation
  - Positive thinking (not strong enough)
  - Distractions
  - Venting
  - Blaming others/circumstances
- What does work: expressive writing. Write about your troubles
- General instructions
  - Write 15-20 min/day for 4-5 consecutive days
  - Topic should be personal and important
  - Write continuously. Don't worry about punctuation, spelling, grammar. If you
    run out of things to say, repeat what you have written. Keep pen on paper.
  - Write only for yourself. Destroy or hide what you are writing. Do not turn
    the exercise into a letter. The result is for your eyes only.
  - If you feel you cannot write about something because it will push you over
    the edge, STOP!
  - Some feel sad after writing, especially on the first day. This feeling
    usually goes away in an hour or so
- **Pen and paper work best**, but typing or voice recording are OK
- Writing before stressful situations (e.g. test taking, presentations, surgery,
  ...) can also be beneficial

## [If (domain logic) then CQRS, or Saga (Udi Dahan)](https://www.youtube.com/watch?v=fWU8ZK0Dmxs)

- Hard deletes are painful as they can lead to cascading deletes (e.g. deleting
  a product may delete user purchases)
- We use soft deletes as a "quick fix" to the cascading delete problem
- But deleting makes a lot of sense in a "private domain", e.g. when a user
  updates the product catalog. We can treat this domain as a sandbox, where the
  user can manipulate data in an easy way
- We need to validate data when we are publishing it from the "private domain"
  to a "public domain" (e.g. so that the customer can see the updated product
  catalog)
- Deletes in a "public domain" hide business intent. Why do you want to delete
  data? Do you really want to delete this product, or do intent to no longer
  sell this product?
- Systems like Amazon are a collaborative domain. Checking invariants is doomed
  to be full of race conditions. Example: A user adds a product to his shopping
  cart. An employee marks the same product as "not for sale". Depending on the
  timing of these requests, an invariance such as "a user cannot buy an item if
  it is not for sale" cannot hold.
- We need to deal with eventual consistency in the context of the business.
  Don't confuse this with technical eventual consistency (e.g. updating read
  models)

## [Cultivating Architecture (Martin Fowler, Birgitta Böckeler)](https://www.youtube.com/watch?v=MZnrxjw602E)

- Good architecture can accelerate a team as it can keep the cost of change down
- Software delivery performance correlates with organizational performance
- Strive to create autonomous teams
- Inform technical staff about the business goals
- Create a set of guiding principles which should help a team when dealing with
  architecture decisions
  - Find principles by identifying what's moving you forward and what's holding
    you back
- Create your own tech radar. What technology do we use? What do we want to try?
  What do we want to get rid of?
- Document any decisions. A simple markdown file might be enough

## [What I learned from three years of sciencing the crap out of DevOps (Jez Humble)](https://www.youtube.com/watch?v=kr08zSnyU20)

- Job satisfaction is the biggest indicator for organization performance
- IT companies with high throughput perform better in terms of stability

## [Files (Dan Luu)](https://www.deconstructconf.com/2019/dan-luu-files)

- We believe that file systems are a solved problem and that they share a common
  abstraction, but that is not true
- Writing a file may seem easy, but there's a lot that could go wrong. File
  systems have bugs too
- File operations may not be atomic
- Even great programmers make mistakes when using the file system. Static
  analysis tools found bugs when inspecting code bases such as Git, Postgres
- Sqlite is a rather stable way to interact with the file system
- Different file systems have different behavior when dealing with errors
- SSDs need ECC (error correcting codes) not be "better", but rather to work at
  all
- Computers don't work

## [Optimize For Time (Andy Walker)](https://www.youtube.com/watch?v=fBr8BKPW5tc)

- High performing teams seem to have more time to get things done. They don't
  hurry. They hit their deadlines
- Struggling teams seem to always be behind
- Busyness is a curse
- Four things he holds true
  - Invest in improvement
  - Respect each other's time
  - Ruthless about time (say no to things that don't make sense)
  - Anticipate problems
- What if the team is the product? If you are not investing in moving faster
  you're moving slower
- Only interrupt people if there is an important reason to do so
- Teams that invest in each other achieve more
- Change is expensive! Fail fast
- When given a hard deadline, work from the basis that everything is going to
  failure
  - Plan for failure
  - Plan to fail cheaply
  - Your plan is not the outcome
  - Recover quickly

## [Conversational Transformation (Jeffrey Fredrick, Douglas Squirrel)](https://www.youtube.com/watch?v=RMT_Tqzf_vc)

Conversational Analysis with The 4 Rs

- Record
  - Fold a piece of paper in half. Write the major points of your conversation
    on the right hand side. Record what you thought (but didn't say) on the left
    hand side
- Reflect
  - How many genuine questions were asked?
  - What is on the left side that isn't on the right?
  - What sets off negative reactions for you?
- Revise
- Roleplay
- (Repeat)
- (Role-reversal)

## [Continuous Retrospectives (Linda Rising)](https://www.youtube.com/watch?v=1wzedZcSmKY)

- In times like COVID we cannot even remember what day it is. How can we then
  have a meaningful discussion (retrospective) about a long project?
- Continuous retrospectives: Hang up a timeline and add sticky notes through out
  the day. Capture ideas, questions, concerns, events, problems, success,
  failure
- Spend the last 15 minutes writing about, reflecting on lessons learned that
  day
- Guide Boards
  - https://www.youtube.com/watch?v=DnZnb22Cgd8
  - https://mattischneider.fr/agile/guide-board/paper.pdf
  - https://mattischneider.fr/anthropologie/agile/thesis
- Retrospectives offer different opportunities:
  - Project: long term learning (strategic)
  - Iteration: what should we do now? (tactical)
  - Continuous: small experiments

## [Solving Problems the Clojure Way (Rafal Dittwald)](https://www.youtube.com/watch?v=vK1DazRK_a0)

- Imperative code spreads state, mutation and side effects, which makes larger
  programs harder to understand and change
- Object oriented programming tries to solve these problems through classes and
  encapsulation. The preferred thinking model revolves around agents and how
  they communicate with each other
- While we cannot get rid of state, functional programming uses a set of
  techniques to avoid state wherever possible. Rafal outlines a few techniques:
  - **Minimize** state
      - Derive state from other state (e.g. the current player of a Tic Tac Toe
        game can be derived based on the board state)
      - Use immutable data structures instead of mutation
      - Pass lambdas
      - Recursion
  - **Concentrate** state into fewer places
  - **Defer** actions (e.g. Elm architecture)
- Given a graph of components, the typical OO approach is to keep state
  separated by pushing it down as far as we can. The FP approach would be to put
  all the state into the root node

## [Persistent Data Structures and Managed References (Rich Hickey)](https://www.infoq.com/presentations/Value-Identity-State-Rich-Hickey/)

- Pure functions have no notion of time and no effect on the world
- Concurrency breaks variables badly
  - Might not be atomic (e.g. long)
  - Need locks or volatile keywords
- Identity: An entity we associate with a series of relates values over time
  - Can be a composite (e.g. the members of a sports team might change, but we
    still consider it to be the same sports team)
- State: Value of an identity at a time
- Value: An immutable structure (e.g. numbers, strings, ...)
- Overall philosophy
  - Things don't change in place
  - See time as a dimension
  - The future is a function of the past (and doesn't change it)
  - Co-located entities can observe each other without cooperation

## [End to end functional tests that can run in milliseconds (Nat Pryce)](https://www.youtube.com/watch?v=Fk4rCn4YLLU)

- They applied the hexagonal architecture model in combination with
  "screenplays"
- Tests can run in different scenarios (in memory, using a Browser with or
  without JS, REST calls, ...)
- They put all interactions (e.g. steps a user takes to update his mail address)
  behind an interface, so that these "use cases" don't know anything about a
  scenario. This technique allows the team to change an N*M mapping to an N+M
  mapping
- Gives great feedback about the actual state of the system. Such an approach
  can find problems in your CDN configuration or your caching policies
- Makes the overall system more observable
- To test/maintain a system we need to
  - Know what the system is doing
  - Know when it has stopped doing it
  - Know when the system has failed
  - Explain what has gone wrong
  - Restore the system to a good state

## [Design Microservice Architectures the Right Way (Michael Bryzek)](https://www.youtube.com/watch?v=j6ow-UemzBc)

- Describe APIs/Events/Databases (e.g. by using JSON) and invest in tooling
  - Create custom linters to ensure that common naming conventions are used
  - Use code generation to automate API creation using CI/CD
  - Use code generation to create mocks
  - Create databases on the fly
- Each microservice owns its own database. Other services use APIs + Events
- Event principles:
  - Producers guarantee at least once delivery
  - Consumers implement idempotency

Key points:

- Design schema first for all APIs and Events
  - Consume Events (not APIs) by default
- Invest in automation
  - Deployment, code generation, dependency management
- Enable teams to write amazing and simple tests
  - Drives quality, streamlines maintenance, enables continuous delivery
