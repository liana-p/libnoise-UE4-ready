# libnoise UE4-ready

libnoise configured for x64 build compatible with UE4, based on the project [LaithS/libnoise](https://github.com/LaithS/libnoise). It can work outside of UE4, but the purpose of this repo is to make it available for UE4.

Currently updated for Visual Studio 2017, compatible with the latest Unreal versions.

See the original [libnoise library](http://libnoise.sourceforge.net/downloads/index.html).

This project is intended to target x64, the reason is to use in Unreal Engine 4, in which you need to [compile libraries yourself](https://wiki.unrealengine.com/Linking_Static_Libraries_Using_The_Build_System) using the same compiler as your game

## Compiling

* You need Visual Studio 2017, and you need to have the [Microsoft Foundation Classes](https://stackoverflow.com/a/43075169) for it to compile.
* Open the solution, check that the target is Build for x64, and build

Once you have built the .dll, you can [include the library](https://wiki.unrealengine.com/Linking_Static_Libraries_Using_The_Build_System) in your Unreal project.