# Usage

Using this framework is really simple.

##Basic usage

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
Add ``RebuildDependencyListOnSceneChange`` to easily rebuild dependency list when scene is unloaded or manually call ``{{project.codename}}.RebuildDependencyList()``

## In-editor injection
This feature saves performance by performing injection in-editor instead at runtime. Dependencies are injected once in-editor instead of everytime when entering playmode or starting game.
<br>

- First you need to have ``LoadDependencies`` component in one of your gameobjects (gameobject shouldn't be marked by ``[DontDestroyOnLoad]`` or moved to different scene)
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

Example for in-editor injection of named dependency ``ClassNamedDependency``:

```csharp
[AutoInjectInEditor]
public class InjectTest : MonoBehaviour
{
    [Inject("ClassNamedDependency", true)]
    public ClassNamedDependency ClassNamedDependency;
}
```

<br>

MicroInject also allows injecting only one named dependency by calling ``{{project.codename}}.InjectNamedDependency("NameOfField", component)``.

Example for named dependency ``ClassNamedDependency``:

```csharp
public class InjectTest : MonoBehaviour
{
    [Inject("ClassNamedDependency")]
    public ClassNamedDependency ClassNamedDependency;
    
    void Start()
    {
        {{project.codename}}.InjectNamedDependency("ClassNamedDependency", this);
    }
}
```

## <b>Preview</b> Dynamically named dependencies

<b>TODO</b>