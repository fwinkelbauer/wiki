# TDODAR

Source: [Pilot Decision Management - Clifford Agius](https://www.youtube.com/watch?v=QNA9EExd8lQ).

## Time

- Is it an emergency? Do we have to act quickly?
- Do we need to and can we make more time?
- Do we have time for a cup of coffee?
- Start a stopwatch and make sure you come back to the "T" to check if things
  change

## Diagnosis

What do we think happened?

- Discuss the symptons
- Ask open questions
- Find some information to tell me this is not XYZ
- Agree on the issue to be tackled
- Make it quick and concise, the clock is ticking!

## Options

What should we do?

- Brainstorm possible options
- Tell people "Give me another option!" if the discussion dries up
- Take input from all members of the team and outside sources
- No such thing as a silly idea. Verbalise everything
- Don't drag it out, be quick. Often the first ideas are the best anyway

## Decide - What are we going to do?

- As a team decide what is the correct or chosen path
- Don't spend too much time deciding, pick an option and go with it
- **State the decision**

## Assign

Assign tasks to the members of the team

- Team leader assigns tasks
- Make tasks short and within the skills of that team member
- It is not a race!
- Complete your task as well as you can but don't delay completion
- If you can't complete ask for help
- Consider overload

## Review

- Has the issue been resolved?
- Do we still have time?
- Quickly repeat TDODAR to see if actions have changed the answer
- Is it still a good decision?

# Speakers

- Kevlin Henney
- Rich Hickey
- Scott Hanselman
- Greg Young
- Michael Feathers
- Dan North
- Martin Fowler
- Martin Thompson
- Bryan Cantrill
- Mark Seemann
- Jimmy Bogard
- Hadi Hariri
- Jonathan Blow
- Simon Brown
- Sam Newman
- J. B. Rainsberger

# The Tao of Coaching (Max Landsberg)

|               | **Low Skill** | **High Skill** |
| ------------- | ------------- | -------------- |
| **High Will** | Guide         | Delegate       |
| **Low Will**  | Direct        | Excite         |


# Project Checklist

- [ ] No sensitive data in SCM
- [ ] Meta data up-to-date
  - Corporate identity
  - Copyright
  - Current version number (or SCM id)
- [ ] Changelog up-to-date
- [ ] Projects use code analysis, auto-formatter and style checks
- [ ] Pipeline warnings are treated as errors
- [ ] No artifacts are stored in SCM

# CTO

As a CTO, my default loop is "First, cycle through all my employees and make
sure that I have equipped them to be happy and productive in their jobs. Second,
find something to do. If possible, delegate it; if not, do it. Repeat."

# Decision Logs

Create logs that hold the following information:

- Problem you are deciding on
- Invovled people
- What did you choose? Why? Risks?
- What other things have you considered? Why didn't you choose them?

# Backups

Backups don't mean anything unless you have actually tried to restore data from
them.

# Modularity

"Start with a list of difficult design descisions or design decisions which are
**likely to change**. Each module is then designed to **hide such a decision** from
the others." - David Parnas

The goal of modulariy is not to make things easier but to hide the things that
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
- Try to initialise resources if they are not found (e.g. create directories,
  create files, ...)
- Create application arguments which allows a user to specify a configuration
  file in a specific place
- Some configuration (e.g. plugging an application into ElasticSearch) should be
  disabled by default. A user can enable them if he wants to

# Become a Software Craftsman

Based on Dan North's talk "[Sharpening the Tools](https://www.infoq.com/presentations/Sharpening-the-Tools)"

## Novice Programmer

Need rules (**not patterns**) to guide their way: Don't ask. Follow this advice
and you will be fine.

## Advanced Beginner

Do not follow the rules! Find out why the rules are the rules. We are starting
to get context - we experience how stuff works.

## Competent

You become goal oriented. This is a time based thing. Most people become
competent if they keep doing something.

## Proficient

This is a deliberate step. Things start to become intuition. Patterns start to
become useful. "How can I make this better?"

## Expert

You are operating of instinct. You don't think about rules, you "just know".
This is critical: You don't know how you come up with your decision.

# 5 Whys

Wikipedia:

> 5 Whys is an interative interrogative technique used to explore the
> cause-and-effect relationships underlying a particular problem. The primary
> goal of the technique is to determine the root cause of a defect or problem by
> repeating the question "Why".

# SBI Model

- **Situation:** In the team meeting on friday
- **Behaviour:** you spoke across me several times
- **Impact:** so I felt like I wasn't being allowed to share my opinion with the
  team

# Visual Studio

## Style

- Import `solarized.vssettings`
- Increase font size

## Extension

- [Match Margin](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.MatchMargin)

## Hotkeys

### Edit

| Shortcut            | Description          |
| ------------------- | -------------------- |
| Ctrl + Backspace    | Delete word to start |
| Ctrl + Del          | Delete word to end   |
| Shift + Alt + Enter | Fullscreen           |
| Ctrl + Shift + V    | Clipboard            |

### Navigate

| Shortcut               | Description                         |
| ---------------------- | ----------------------------------- |
| Ctrl + G               | Go to line                          |
| Ctrl + ,               | Navigate to input                   |
| Ctrl + Shift + Up/Down | Previous/next highlighted reference |

### Code

| Shortcut             | Description                    |
| -------------------- | ------------------------------ |
| Ctrl + K, Ctrl + S   | Surround with snippet          |
| Ctrl + K, Ctrl + C   | Comment out selection          |
| Ctrl + K, Ctrl + U   | Uncomment selection            |
| Ctrl + .             | Open IntelliSense context menu |
| Ctrl + R, Ctrl + R   | Rename                         |
| Ctrl + Shift + Space | Show parameter info            |

### Other

| Shortcut         | Description          |
| ---------------- | -------------------- |
| Ctrl + Shift + B | Build solution       |
| Ctrl + R, B      | Run all tests        |
| F5               | Run / Continue       |
| F10              | Step over            |
| F11              | Step into            |
| Shift + F11      | Step out             |
| F12              | Go to definition     |
| Ctrl + F12       | Go to implementation |
| Shift + F12      | Find all references  |
| Ctrl + Q         | Quick launch         |

# Test Doubles

Explained by [Uncle
Bob](https://blog.cleancoder.com/uncle-bob/2014/05/14/TheLittleMocker.html):

``` java
interface Authorizer {
  public Boolean authorize(String username, String password);
}
```

## Dummy

``` java
public class DummyAuthorizer implements Authorizer {
  public Boolean authorize(String username, String password) {
	return null;
    }
}
```

Use it if the object is never used. The `NullPointerException` can help you to
insure such thing.

## Stub

``` java
public class AcceptingAuthorizerStub implements Authorizer {
  public Boolean authorize(String username, String password) {
    return true;
  }
}
```

A stub can help to reduce coupling.

**Example:** With this stub your test does
not need to care about the actual login operation. It can behave as if it was
already logged in.

## Spy

``` java
public class AcceptingAuthorizerSpy implements Authorizer {
  public boolean authorizeWasCalled = false;

  public Boolean authorize(String username, String password) {
    authorizeWasCalled = true;
    return true;
  }
}
```

The more you spy on an object, the tighter the coupling between test and
implementation becomes.

## Mock

```java
public class AcceptingAuthorizerVerificationMock implements Authorizer {
  public boolean authorizeWasCalled = false;

  public Boolean authorize(String username, String password) {
    authorizeWasCalled = true;
    return true;
  }

  public boolean verify() {
    return authorizedWasCalled;
  }
}
```

A mock knows how it is supposed to be used.

## Fakes

``` java
public class AcceptingAuthorizerFake implements Authorizer {
  public Boolean authorize(String username, String password) {
    return username.equals("Bob");
  }
}
```

Fakes have business behaviours. Think of it as a simulator. Uncle Bob claims,
that he has not written any fake object in over 30 years. Be careful.

## Summary

Uncle Bob:

> A Mock is a kind of spy, a spy is a kind of stub, and a stub is a kind of
> dummy. But a fake isn't a kind of any of them. It's a completely different
> kind of test double.

# Meetings

[Video](https://www.youtube.com/watch?v=ppfLUFO-hwc):

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

# Cynefin Framework

- **Chaotic:** A crisis. Your best bet is to stabilize the situation
- **Complex:** (Creativity) You don't know what you don't know, so you have to try things out
  to understand your environment
  - Make the right system
- **Complicated:** (Skill) You kinda know what's going on so you can engineer a solution
  - Make the system right
- **Obvious:** (Automation) You can automate these steps
