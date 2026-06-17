## How Unreal builds your game

Because Unreal Engine uses C++ instead of an interpreted language like Python or GDScript, it must be compiled at some point. Your game code that lives on top needs to be combined with the engine code underneath in order for it to be properly understood by the compiler - so it can make your finished Windows or Linux executable.

This process of compilation can easily be initiated by clicking the Live Coding button at the bottom right corner of the Unreal Editor, or by pressing _Ctrl+Alt+F11_. You should do this any time you make changes to a C++ file.
If this is causing issues, it may be worth saving your work and closing the Unreal Editor, going back to Visual Studio and pressing Build -> Rebuild, then opening the Unreal Editor again.

Most of what is explained below is written for you if you generate your C++ classes within the Unreal Editor, but it's still good info.

## Unreal Header Tool (UHT) 🥛

Just before your game is compiled, the Unreal Header Tool program scans all the header files in your project and generates additional _generated.h_ and _.generated.cpp_ files alongside your own. This is essentially the glue that connects your game code to the Unreal engine code.

This is why Unreal adds lines like these to your header files:
```
#include "MyClass.generated.h"
```
That generated.h file is explicitly created by UHT just before compile time and isn't very human readable. You aren't supposed to edit it, and it's more there so the engine can easily combine all your source code together.
It's worth mentioning that the _.generated.h_ include needs to be the very last thing included at the top of your class's header file.

## The GENERATED_BODY() macro

Every UObject-derived class you write (which will be most of them) must contain one GENERATED_BODY() call at the top of its definition. For example:
```
class MYGAME_API APlayerCharacter : public ACharacter
{
    GENERATED_BODY()
    ...
}
```
When the UHT sees this in your code, it replaces it with a large block of auto-generated code that allows your class to interact with the lower-level Unreal ones. You never write or even read that generated code, but just know that it must be there for everything to work properly.

## The MODULE_API macro
Unreal compiles your game code into a library (DLL on windows and .so on Linux), and for this it expects your game to be contained within one "module".
For this to work, every class in your game must integrate the "MODULE_API" macro. This is written as ```THENAMEOFYOURGAME``` followed by ```_API```. For example, if you were writing a PlayerCharacter class in a game project called MyGame:
```
class MYGAME_API APlayerCharacter : public ACharacter
```
Again, you needn't really understand the nitty-gritty specifics of how this works, just know that each class in your game you'll need to include this macro at the beginning.

## Why is my class not showing up in the Unreal Editor? Is it BORKED??? The code makes sense...

Don't fret! It (probably) isn't your fault! It may be the result of a "stale build". This is where, for whatever reason, the UHT isn't creating the _.generated_ files for your class properly.

This can usually be resolved by closing the Unreal Editor, going to Visual Studio and pressing _Build -> Clean_ and then _Build -> Rebuild_. In a worst-case scenario, you might want to try deleting the _.vs_, _Binaries_, _DerivedDataCache_, _Intermediate_ and _Saved_ folders in your project, and potentially right click the _.uproject_ file and generate Visual Studio project files again.

[Next page](https://skullsoftr.github.io/ActuallyGoodUnrealDocs/api/cpp/uclass.html)
