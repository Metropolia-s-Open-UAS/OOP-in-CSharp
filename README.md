#### OOP in CSharp
 software design Paradigm that builds programs around "objects"—which combine data (attributes) and code (methods)—rather than standalone functions.
#### Object-Oriented Programming in C#

#### The four pillars

| Pillar | Idea | C# mechanism |
|---|---|---|
| **Encapsulation** | Hide internal state and expose only what's needed | `private`/`protected`/`public`, properties |
| **Inheritance** | Reuse and extend an existing class | `class Child : Parent` |
| **Polymorphism** | Same call, different behavior | `virtual`/`override`, interfaces |
| **Abstraction** | Expose *what* an object does, not *how* | `abstract` classes, `interface` |

#### Access modifiers

| Modifier | Visible to |
|---|---|
| `public` | Everywhere |
| `private` | Same class only (default for members) |
| `protected` | Same class and derived classes |
| `internal` | Same assembly (default for top-level types) |
| `protected internal` | Same assembly or derived classes |
| `private protected` | Derived classes within the same assembly |

#### Example

```csharp
// Abstraction + Encapsulation
public abstract class Device
{
    private string _serial;                 // encapsulated field

    public string Serial                    // property with validation
    {
        get => _serial;
        set => _serial = string.IsNullOrWhiteSpace(value)
            ? throw new ArgumentException("Serial required")
            : value;
    }

    protected Device(string serial) => Serial = serial;

    public abstract string Describe();      // must be overridden
    public virtual void PowerOn() => Console.WriteLine("Powering on...");
}

// Inheritance
public class Monitor : Device
{
    public Monitor(string serial) : base(serial) { }

    public override string Describe() => $"Patient monitor #{Serial}";
    public override void PowerOn()          // Polymorphism
    {
        base.PowerOn();
        Console.WriteLine("Calibrating sensors...");
    }
}

// Interface
public interface IServiceable
{
    void Service();
}

public class Ventilator : Device, IServiceable
{
    public Ventilator(string serial) : base(serial) { }
    public override string Describe() => $"Ventilator #{Serial}";
    public void Service() => Console.WriteLine("Servicing ventilator");
}

// Usage: polymorphism in action
List<Device> devices = new() { new Monitor("M-100"), new Ventilator("V-200") };
foreach (var d in devices)
{
    Console.WriteLine(d.Describe());
    d.PowerOn();
}
```

#### Key keywords

| Keyword | Meaning |
|---|---|
| `abstract` | Class can't be instantiated, or a method has no body and must be overridden |
| `virtual` | Method can be overridden in derived classes |
| `override` | Replaces a virtual/abstract base method |
| `sealed` | Prevents further inheritance or overriding |
| `static` | Belongs to the type, not an instance |
| `base` | Refers to the parent class |
| `this` | Refers to the current instance |
| `new` (on member) | Hides a base member (avoid; it isn't true polymorphism) |

#### Abstract class vs. interface

| | Abstract class | Interface |
|---|---|---|
| Multiple inheritance | No (one base class) | Yes (implement many) |
| Fields / state | Yes | No |
| Constructors | Yes | No |
| Default implementation | Yes | Yes (C# 8+ default methods) |
| Best for | Shared base behavior ("is-a") | Capabilities/contracts ("can-do") |

#### Modern C# features that complement OOP

| Feature | Example |
|---|---|
| Auto-properties | `public string Name { get; set; }` |
| `init`-only setters | `public string Id { get; init; }` |
| Records (immutable data types) | `public record Product(string Name, decimal Price);` |
| Primary constructors (C# 12) | `public class Svc(ILogger log) { }` |
| Required members | `public required string Name { get; set; }` |
| Generics | `class Repo<T> where T : class` |

#### Rules of thumb

| Principle | Guidance |
|---|---|
| Composition over inheritance | Prefer holding a dependency over deep inheritance trees |
| Program to interfaces | Depend on `IRepository`, not a concrete class |
| SOLID | Single responsibility, Open/closed, Liskov, Interface segregation, Dependency inversion |

