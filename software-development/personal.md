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
