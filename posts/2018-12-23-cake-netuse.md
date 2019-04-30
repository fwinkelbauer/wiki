# Copy Files to a Remote Location Using Cake

I have recently had the need to copy files from one Windows machine to another
in a Cake build script task. A rather easy way to achieve this is to use the
`net` command line tool, which I have wrapped into a few lines of C#. Here's how
you use the code:

```csharp
readonly var target = Argument("target", "Default");

Task("Default")
    .Does(() =>
{
    using (var session = NetUse("\\\\some-machine", "domain\\user", "password"))
    {
        // Use Cake's existing IO library
        CopyFileToDirectory("myfile.txt", session.ToRemote("C:/"));
    }
});

RunTarget(target);
```

The two most important bits include:

- Opening a session using the `NetUse` method
- Using the `session.ToRemote` method to turn a "normal" path into a remote
  path (e.g. `C:\` -> `\\some-machine\c$\`)

Here's the actual `NetUse` implementation:

```csharp
public NetUseSession NetUse(string machine, string username, string password)
{
    Information($"Connecting to '{machine}'");
    var exitCode = StartProcess("net", new ProcessSettings
    {
        Arguments = new ProcessArgumentBuilder()
            .Append("use")
            .Append(machine)
            .AppendSecret(password)
            .Append($"/user:{username}")
    });

    if (exitCode != 0) {
        throw new Exception($"Could not connect to '{machine}'");
    }

    Func<string, int> closeFunc = (m) =>
    {
        Information($"Deleting connection to '{m}'");
        return StartProcess("net", new ProcessSettings
        {
            Arguments = new ProcessArgumentBuilder()
                .Append("use")
                .Append(m)
                .Append("/delete"),
        });
    };

    return new NetUseSession(machine, closeFunc);
}

public sealed class NetUseSession : IDisposable
{
    private readonly string _machine;
    private readonly Func<string, int> _close;

    public NetUseSession(string machine, Func<string, int> close)
    {
        _machine = machine;
        _close = close;
    }

    public DirectoryPath ToRemote(DirectoryPath directory)
    {
        var remoteDirectory = directory.ToString().Replace(":", "$");
        return $"{_machine}/{remoteDirectory}";
    }

    public void Dispose()
    {
        var exitCode = _close(_machine);

        if (exitCode != 0) {
            throw new Exception($"Could not delete connection to '{_machine}'");
        }
    }
}
```
