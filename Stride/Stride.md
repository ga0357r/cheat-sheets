# Stride/Xenko
- [Stride Manual](https://doc.stride3d.net/latest/en/manual/)
- [Stride for Unity developers](https://doc.stride3d.net/latest/en/manual/stride-for-unity-developers/index.html)

## Similarities with Unity
Unity -> Stride
Hierarchy panel -> Entity Tree
Inspector -> Property Grid
Project browser -> Asset View
Scene view -> Scene Editor
GameObject -> Entity
MonoBehaviour -> SyncScript, AsyncScript, StartupScript

## Folders and Files
the project .sln solution file, which you can open with Game Studio or any IDE such as Visual Studio

Bin contains the compiled binaries and data. Stride creates the folder when you build the project, with a subdirectory for each platform.

MyPackage.Game contains your source code.

MyPackage.Platform contains additional code for the platforms your project supports. Game Studio creates folders for each platform (eg MyPackage.Windows, MyPackage.Linux, etc). These folders are usually small, and only contain the entry point of the program.

obj contains cached files. Game Studio creates this folder when you build your project. To force a complete asset and code rebuild, delete this folder and build the project again.

Resources is a suggested location for files such as images and audio files used by your assets.

## Game Settings 
Stride saves global settings in a single asset, the Game Settings asset. You can configure:

- The default scene
- Rendering settings
- Editor settings
- Texture settings
- Physics settings
- Overrides

To use the Game Settings asset, in the Asset View, select GameSettings and view its properties in the Property Grid.

## Scenes
Like Unity®, in Stride you place all objects in a scene. Game Studio stores scenes as separate .sdscene assets in your project directory.

## Entities
Like GameObjects, entities are carriers for components such as transform components, model components, audio components, and so on. If you're used to working with GameObjects in Unity®, you should have no problem using entities in Game Studio.

## Entity Component
In Stride, you add components to entities just like you add components to GameObjects in Unity®. You use the property grid to add components to an entity.

### Transform component
In Stride, Transform components contain a LocalMatrix and a WorldMatrix that are updated in every Update frame.

For local use Transform.Position, Transform.Rotation, Transform.Scale, Transform.RotationEulerXYZ.

For world position use Transform.worldMatrix.TranslationVector, 
Transform.WorldMatrix.DecomposeXYZ(out Vector3 rotation)
Transform.WorldMatrix.Decompose(out Vector3 scale, out Vector3 translation)
Transform.WorldMatrix.Decompose(out Vector3 scale, out Quaternion rotation, out Vector3 translation)

### Transform directions
Transform.WorldMatrix.Forward
Transform.WorldMatrix.Backward
Transform.WorldMatrix.Right
Transform.WorldMatrix.Left
Transform.WorldMatrix.Up
Transform.WorldMatrix.Down

## Archetypes
Archetypes are master assets that control the properties of assets you derive from them.

## Input
```
public override void Update()
{
    // true for one frame in which the space bar was pressed
    if(Input.IsKeyDown(Keys.Space))
    {
        // Do something.
    }

    // true while this joystick button is down
    if (Input.GameControllers[0].IsButtonDown(0))
    {
        // Do something.
    }

    float Horiz = (Input.IsKeyDown(Keys.Left) ? -1f : 0) + (Input.IsKeyDown(Keys.Right) ? 1f : 0);
    float Vert = (Input.IsKeyDown(Keys.Down) ? -1f : 0) + (Input.IsKeyDown(Keys.Up) ? 1f : 0);
    //Do something else.
}
```
