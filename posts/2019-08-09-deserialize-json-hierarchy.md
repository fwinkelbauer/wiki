# Deserialize JSON to Concrete Type in C Sharp

Let's imagine that we are building a REST API. One of our endpoints accepts
"commands", which might look like this:

``` json
{
    "TypeName:": "SomeCommand",
    "SomeText": "Foo"
}
```

or like this:

``` json
{
    "TypeName:": "OtherCommand",
    "SomeNumber": 5
}
```

Which could map to these classes:

``` c#
public abstract class AbstractCommand
{
    public string TypeName => GetType().Name;
}

public class SomeCommand : AbstractCommand
{
    public SomeCommand(string someText)
    {
        SomeText = someText;
    }

    public string SomeText { get; }
}

public class OtherCommand : AbstractCommand
{
    public OtherCommand(int someNumber)
    {
        SomeNumber = someNumber;
    }

    public int SomeNumber { get; }
}

```

The following code snippet will deserialize the JSON examples into their
concrete types:

``` c#
public static AbstractCommand Deserialize(string content)
{
    var jObject = JObject.Parse(content);
    var typeName = jObject.Property(nameof(AbstractCommand.TypeName)).Value.ToString();

    var baseType = typeof(AbstractCommand);
    var childType = Type.GetType($"{baseType.Namespace}.{typeName}, {baseType.Assembly}");

    return (AbstractCommand)jObject.ToObject(childType);
}
```
