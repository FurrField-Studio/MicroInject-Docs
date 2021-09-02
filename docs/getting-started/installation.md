# Installation

{{project.name}} is simple Dependency Injection framework made for Unity Engine that allows you to easily inject components.

### Using OpenUPM (Recommended) (Needs to publish to OpenUPM)

Add the OpenUPM registry with the ``com.furrfield`` scope to your project
<br>

- Open ``Edit/Project Settings/Package Manager``
- Add a new Scoped Registry:
```
Name: OpenUPM
URL:  https://package.openupm.com/
Scope(s): com.furrfield
```
- Click save
<br>

Add this package:

- Open ``Window/Package Manager``
- Click ``+``
- Click ``Add package from git URL`` or ``Add package by name``
- Paste com.furrfield.micro-inject
- Click ``Add``


### Using package (Recommended)

- Open ``Window/Package Manager``
- Click ``+``
- Click ``Add package from git URL`` or ``Add package by name``
- Add ``https://github.com/FurrField-Studio/MicroInject.git`` in Package Manager

### Using AssetStore (No Samples available) (Needs to publish to assetstore)

### Using .unitypackage (No Samples available)

- Go to ``https://github.com/FurrField-Studio/MicroInject/releases`` and download latest ``MicroInject.unitypackage``
- Import it to your project