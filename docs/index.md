MicroInject is simple Dependency Injection framework made for Unity Engine that allows you to easily inject components.
<br>
<br>
Features:
<br>

- Runtime injection
- In-editor injection - injecting dependencies without any runtime performance impact
- Named dependencies - injecting dependencies that can be named (Useful in multiplayer projects when we want to inject Player1 components first and Player2 components when Player1 dies, to see Player2 stats)
- (<b>Preview</b> not finished) Dynamically named dependencies - the same as above but names can change during runtime (Useful when we want to inject specific player data based on a name during runtime)

<br>
Roadmap:
<br>

- <b>(Critical for release)</b> Name tracking for dynamically named dependencies (this should simplify usage of dynamically named dependencies)
- Pre-register injection marked fields for dynamically registered dependencies - injecting ``[DontDestroyOnLoad]`` dependencies or dependencies from other scenes without using the entire runtime injection loop