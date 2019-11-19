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

Fakes have business behaviors. Think of it as a simulator. Uncle Bob claims,
that he has not written any fake object in over 30 years. Be careful.

## Summary

Uncle Bob:

> A Mock is a kind of spy, a spy is a kind of stub, and a stub is a kind of
> dummy. But a fake isn't a kind of any of them. It's a completely different
> kind of test double.

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
