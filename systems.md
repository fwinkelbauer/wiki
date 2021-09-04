# Systems

## Antifragility (Nassim Nicholas Taleb)

An antifragile system benefits from disorder

- Have options (instead of obligations)
- Create a situation with low downside and high upside
- Create systems with slack

### Nonlinear events

- Convex: a smiling graph. More potential upside than downside (positive Black
  Swan)
- Concave: a frowning graph. More potential downside than upside (negative Black
  Swan)
- You are robust if doubling exposure doubles harm
- You are fragile if doubling exposure more than doubles harm
- A single 100 pound stone hurts more than 100 one pound stones

### Example

Traffic is a function, which depends on the amount of cars. This function is
nonlinear (concave), as the following two situations create different outputs:

- One hour of 8.000 cars, followed by one hour of 12.000 cars
- Two hours of 10.000 cars

**Important:** Don't confuse x with f(x).

In general:

- A function is concave if the average function value is lower than the
  function of the average value
- A function is convex if the average function value is higher than the function
  of the average value

## Meltdown (Chris Clearfield, András Tilcsik)

**Why our systems fail and what we can do about it**

Complexity and coupling increase the chance of big errors. In these systems,
small problems and minor issues can accumulate into an unforeseen meltdown.

In the past, nuclear power plants were one of only a few systems which were both
complex and highly coupled. Over the last years more and more systems entered
this domain.

How to deal with these systems:

- Introduce slack
- Reduce moving parts
- Replace indirect feedback with direct feedback
  - Example of indirect feedback: An indicator light that shows that a valve was
    instructed to close
  - Example of direct feedback: An indicator light which shows the actual state
    (open/closed) of a valve

Techniques:

- Aviation and medical institutions use "near miss reports" to deal with small
  issues before they can spiral out of control
- Use hindsight bias by performing a premortem. Imagine that the project is
  completed and that it was a complete failure. What were the reasons why it
  failed?
- Subjective Probability Interval Estimates (SPIES): Estimate the probability of
  several outcomes

Lessons:

- Charm school (Everyone should be able to express concern)
  - Get attention
  - Express your concern
  - State the perceived problem
  - Propose a solution
  - Ask for agreement
- Soften power cues
  - This makes you more approachable, which can encourage people to speak their
    mind
- Leaders speak last
  - Encourage discussion about different solutions
  - Not encouraging a discussion can be compared to discouraging a discussion

Diverse groups are "better" because group members are more skeptical which in
turn makes it more likely to catch small errors.
