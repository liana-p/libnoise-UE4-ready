# libnoise UE4-ready

libnoise configured for x64 build compatible with UE4, in which you need to [compile libraries yourself](https://wiki.unrealengine.com/Linking_Static_Libraries_Using_The_Build_System) using the same compiler as your game.

The project is based on [LaithS/libnoise](https://github.com/LaithS/libnoise). It can work outside of UE4, but the purpose of this repo is to make it available for UE4.

Currently updated for Visual Studio 2017, compatible with the latest Unreal versions.

See the original [libnoise library](http://libnoise.sourceforge.net/downloads/index.html).

## Compiling

* You need Visual Studio 2017, and you need to have the [Microsoft Foundation Classes](https://stackoverflow.com/a/43075169) for it to compile.
* Open the solution, check that the target is Build for x64, and build

Once you have built the .dll, you can [include the library](https://wiki.unrealengine.com/Linking_Static_Libraries_Using_The_Build_System) in your Unreal project.

## Using in Unreal Engine

Based on the [documentation for linking static libraries](https://wiki.unrealengine.com/Linking_Static_Libraries_Using_The_Build_System)

#### Adding the library to your project

* Compile the library as explained above for x64 and x86, or download this [zipped build](nialna.github.io/libnoise-UE4-ready/bin.zip) with everything you need (but I'd advise building it yourself).
* In your project's root directory, create a `ThirdParty` directory if you don't have it, and in it create a `LibNoise` directory which will contain a `Includes` and `Libraries` folder.
* In the `Libraries` folder, copy `libnoise.lib` and rename it `libnoise.x64.lib` (you can also build and copy `libnoise.x86.lib`).
* In the `Includes` folder, copy all the .h files of libnoise (the content of the `include` directory at the root of this repo).

#### Including the library in your build

Simple instructions to add the library to your Unreal project, if you don't know how to.

Add the following function to your game class `MyProject.Build.cs` file

```cs
public bool LoadLib(ReadOnlyTargetRules Target, string libPath, string libName)
{
    bool isLibrarySupported = false;

    if (Target.Platform == UnrealTargetPlatform.Win64 || Target.Platform == UnrealTargetPlatform.Win32) {
        isLibrarySupported = true;
        string PlatformString = (Target.Platform == UnrealTargetPlatform.Win64) ? "x64" : "x86";
        string LibrariesPath = Path.Combine(ThirdPartyPath, libPath, "Libraries");

        PublicAdditionalLibraries.Add(Path.Combine(LibrariesPath, libPath + "." + PlatformString + ".lib"));
        PublicIncludePaths.Add(Path.Combine(ThirdPartyPath, libPath, "Includes"));
    }
    Definitions.Add(string.Format("WITH_" + libName + "_BINDING={0}", isLibrarySupported ? 1 : 0));
    return isLibrarySupported;
}
```

Now, in the constructor of your game in the same file, you can add a call to add the library at the end:

```cs
public MyGame(ReadOnlyTargetRules Target): base(Target)
{
	// All the default constructor stuff
	loadLib(Target, "LibNoise", "LIB_NOISE"); // LibNoise is the name of the folder where you copied the files previously 
}
```

## Hello World

If you want to verify that the library loads correctly, add the following code to some component so it can run on game start:

```c++
module::Perlin myModule;
double value = myModule.GetValue(1.25, 0.75, 0.50);
UE_LOG(LogTemp, Warning, TEXT("Perlin hello world value: %f"), value);
```