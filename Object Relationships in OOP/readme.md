### Object Relationships in OOP

Aggregation is one of several ways objects relate to each other. Here is the full picture.

#### The relationships

| Relationship | Meaning | Lifetime | Strength | Example |
|---|---|---|---|---|
| **Association** | One object uses or knows another | Independent | Weakest | Doctor ↔ Patient |
| **Aggregation** | "Has-a", the part can exist without the whole | Independent | Weak | Department has Employees |
| **Composition** | "Owns-a", the part dies with the whole | Dependent | Strong | House has Rooms |
| **Inheritance** | "Is-a", child extends parent | n/a | Strongest | Monitor is a Device |
| **Dependency** | One class uses another temporarily (parameter, local variable) | Short-lived | Weakest | Method takes a `Logger` argument |

<img src="https://github.com/Metropolia-s-Open-UAS/OOP-in-CSharp/blob/main/Object%20Relationships%20in%20OOP/oop_relationships_uml.png" alt="UML Image">

#### Aggregation vs. composition

| | Aggregation | Composition |
|---|---|---|
| Ownership | Shared, whole does not own the part | Whole owns the part |
| Part exists without whole? | Yes | No |
| Created by | Passed in from outside | Created inside the whole |
| Same part in many wholes? | Possible | No |
| UML symbol | Hollow diamond ◇ | Filled diamond ◆ |

#### Code example

```csharp
// AGGREGATION: Technician exists independently of the Team
public class Technician
{
    public string Name { get; }
    public Technician(string name) => Name = name;
}

public class ServiceTeam
{
    private readonly List<Technician> _members = new();

    public void Add(Technician t) => _members.Add(t);   // passed in from outside
}

// COMPOSITION: Battery is created and owned by the Device
public class Battery
{
    public int Charge { get; set; } = 100;
}

public class PortableDevice
{
    private readonly Battery _battery = new();          // created inside, dies with owner
}

// Usage
var ali = new Technician("Ali");
var teamA = new ServiceTeam();
var teamB = new ServiceTeam();
teamA.Add(ali);
teamB.Add(ali);          // same technician in two teams
// If teamA is deleted, Ali still exists
```

#### Association example

```csharp
public class Doctor
{
    public void Treat(Patient p) { /* uses patient, doesn't own it */ }
}
public class Patient { }
```

#### Other related OOP concepts

| Concept | Meaning | C# example |
|---|---|---|
| **Coupling** | How dependent classes are on each other (aim for low) | Depend on `IRepo`, not `SqlRepo` |
| **Cohesion** | How focused a class is on one job (aim for high) | A `Invoice` class doesn't send emails |
| **Method overloading** | Same name, different parameters (compile-time polymorphism) | `Add(int a, int b)` and `Add(double a, double b)` |
| **Method overriding** | Derived class replaces base behavior (runtime polymorphism) | `override Describe()` |
| **Dependency injection** | Pass dependencies in rather than creating them | Constructor injection in ASP.NET Core |
| **Delegation** | Forward work to a contained object | `_logger.Log(...)` inside a method |
| **Static vs. instance** | Type-level vs. object-level members | `Math.Max()` vs. `list.Add()` |

#### Quick way to decide

| Question | Answer → Use |
|---|---|
| Is the child a specialized version of the parent? | Yes → **Inheritance** |
| Does the part die when the whole is destroyed? | Yes → **Composition** |
| Can the part live on its own and be shared? | Yes → **Aggregation** |
| Does one class just call another briefly? | Yes → **Dependency / Association** |

