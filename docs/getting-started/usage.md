# Usage

Using this framework is really simple.

##Basic usage

First add ``{{project.name}} Manager`` somewhere in your gameobjects, so system can clear internal data and manage scene switching.

### Marking component as dependency
To use component as a dependency you need to mark its class using ``[Dependency]`` attribute.
<br>
Example:

```csharp
[Dependency]
public class ClassB : MonoBehaviour
{
    //variables

    void Start()
    {
        //code
    }
    
    //code
}
```

then add ``RegisterAsDependencies`` component to gameobject that have components with ``[Dependency]`` attribute.

### Marking field for injection
To inject dependency into a field you need to mark it using ``[Inject]`` attribute, and call ``{{project.codename}}.InjectDependencies(this)`` in ``void Start()``
<br>
Example:

```csharp
public class InjectTest : MonoBehaviour
{
    [Inject]
    public ClassA ClassA;
    
    void Start()
    {
        {{project.codename}}.InjectDependencies(this);
    }
}
```

## Switching scenes
Tick ``RebuildDependencyListOnSceneChange`` in ``{{project.name}} Manager``  to easily rebuild dependency list when scene is unloaded or manually call ``{{project.codename}}.RebuildDependencyList()``

## In-editor injection
This feature saves performance by performing injection in-editor instead at runtime. Dependencies are injected once in-editor instead of everytime when entering playmode or starting game.
<br>

- Add ``LoadDependencies`` component in one of your gameobjects (gameobject shouldn't be marked by ``[DontDestroyOnLoad]`` or moved to different scene)
- Tick ``Register In Editor`` in ``RegisterAsDependencies`` component to mark dependencies in gameobject for in-editor inject.
- Add ``[AutoInjectInEditor]`` attribute to component that should be allowed for in-editor injection and add ``[Inject(true)]`` attribute to any field that should have in-editor injected dependency.
  
Example:
  
```csharp
[AutoInjectInEditor]
public class InjectTest : MonoBehaviour
{
    [Inject(true)]
    public ClassA ClassA;
}
```

- Open MicroInject window located in ``{{studio.name}}/{{project.name}}``
- Click ``Pregenerate dependency list`` to grab in-editor injectable dependencies, then click ``Inject dependencies`` to inject dependencies to fields marked for in-editor injection. (See [{{project.name}} Window](../../window/))

## Named dependencies

### Marking as named dependency

- Add ``[NamedDependencyField]`` attribute to string field located in dependency to mark it as the name of dependency.

Example:
  
```csharp
[Dependency]
public class ClassNamedDependency : MonoBehaviour
{

    [NamedDependencyField]
    public string DependencyName;
    
}
```

<b>Warning</b>:
Don't give the same name to 2 dependencies, because the second dependency will not be registered in the system.

### Marking for named dependency injection

- Add ``[Inject("NameOfDependency")]`` to mark field for named dependency injection.

Example for named dependency ``ClassNamedDependency``:

```csharp
public class InjectTest : MonoBehaviour
{
    [Inject("ClassNamedDependency")]
    public ClassNamedDependency ClassNamedDependency;
    
    void Start()
    {
        {{project.codename}}.InjectDependencies(this);
    }
}
```

### In-editor injection for named dependency

Example for in-editor injection of named dependency ``ClassNamedDependency``:

```csharp
[AutoInjectInEditor]
public class InjectTest : MonoBehaviour
{
    [Inject("ClassNamedDependency", true)]
    public ClassNamedDependency ClassNamedDependency;
}
```

### Manual injection for named dependency

MicroInject also allows manual injection of named dependency by calling ``{{project.codename}}.InjectNamedDependency("NameOfField", component)``.

Example for named dependency ``NamedDependency``:

```csharp
public class InjectTest : MonoBehaviour
{
    [Inject("NamedDependency")]
    public ClassNamedDependency ClassNamedDependency;
    
    void Start()
    {
        {{project.codename}}.InjectNamedDependency("NamedDependency", this);
    }
}
```

## Dynamically named dependencies

Almost like named dependencies but allows to change the name during runtime.
This method doesn't use any attributes instead it uses ``DynamicDependencyName`` class and ``DynamicInject<T>`` generic class to know when Name or Component changes.

### Adding dynamically named dependency

- Add ``DynamicDependencyName`` variable to class that should be dynamically injected.
- Use ``DynamicDependencyName.Name`` variable to change name of dependency.

Example:
  
```csharp
public class DynamicDependency : MonoBehaviour
{
    public DynamicDependencyName DynamicDependencyName;
    
    private void Awake()
    {
        DynamicDependencyName = new DynamicDependencyName(this);
        DynamicDependencyName.Name = "DynamicTest";
    }
}
```

<b>Warning</b>:
Don't give the same name to 2 dependencies, because the dependency with new name will be removed from the system.

### Injecting dynamically named dependency

- Add ``DynamicInject<T>`` variable to your class for dynamically named dependency injection.
- Use ``DynamicInject.Name`` variable to change name of dependency that should be injected.
- And ``DynamicInject.IsInjected`` variable to check if any dependency is injected.

Example for dependency ``DynamicDependency``:

```csharp
public class InjectTest : MonoBehaviour
{
    public DynamicInject<DynamicDependency> DynamicInject = new DynamicInject<DynamicDependency>();
    
    void Start()
    {
        DynamicInject.Name = "DynamicTest";
    }
}
```

<b>Note</b>:
``DynamicInject<T>`` uses internal component variable to store injected dependency, which allows to inject any component, but only component of type `T` can be accessed from user code. This is framework limitation created to prevent developers from directly accessing component and making multiple type checks to get its type.