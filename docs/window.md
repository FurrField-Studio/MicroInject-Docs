# {{project.name}} window

{{project.name}} window for in-editor injecting.

### Pre-generate dependency list
Grabs all dependencies which are on gameobject that have ``Register as dependencies`` component with ticked ``Register in editor`` field, and puts them in dependency list created in ``LoadDependencies`` component.

### Inject dependencies 
Takes list of dependencies from ``LoadDependencies`` component and then injects them to component marked with ``[AutoInjectInEditor]`` attribute. This injection checks every component on scene that is marked by the ``[AutoInjectInEditor]`` attribute, so this could take a while to inject.

### Clear MicroInject lists
This is mostly to clean internal data.

### Debug
Foldout for checking internal lists. What dependencies, named dependencies and dynamic injection fields are registered.