# Entity Component System(ECS)
ECS for Unity (Entity Component System) is a data-oriented framework compatible with GameObjects, enabling seasoned Unity creators to build more ambitious games thanks to an unprecedented level of control and determinism. [To-Do-Information 1](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/index.html). [To-Do-Information 2](https://docs.unity3d.com/Packages/com.unity.entities@1.2/manual/index.html)

The Entity Component System (ECS) is the core of the Unity Data-Oriented Tech Stack. ECS has three principal parts:
1) Entities -  The entities, or things, that populate your game or program.

2) Components - The data associated with your entities, but organized by the data itself rather than by entity. (This difference in organization is one of the key differences between an object-oriented and a data-oriented design.)

3) Systems -  The logic that transforms the component data from its current state to its next state— for example, a system might update the positions of all moving entities by their velocity times the time interval since the previous frame.

## Entities
**Entities represent the individual "things" in you game or program**. An entity has neither behavior nor data; instead, it identifies which pieces of data belong together. Systems provide the behavior. Components store the data. An entity is essentially an ID. You can think of it as a super lightweight GameObject. Entity IDs are the only stable way to store a reference to another component or entity.

An EntityManager manages all of the entities in a World. Although an entity does not have a type, groups of entities can be categorized by the types of the data components associated with them. The EntityManager uses EntityArchetypes to group related entities categorized by the types of the data components associated with them. For more information see [link](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/ecs_entities.html)

### Creating Entities
There are 3 ways to create entities
1) The easiest way is to through the unity Editor - By setting up both GameObjects placed in a scene and Prefabs to be converted into entities at runtime.
2) For more dynamic parts of your game or program - By creating spawning  systems that create multiple entities in a job.
3) Entities can be created one at a time using EntityManager.CreateEntity functions.

Note: I personally will recommend creating entities through the unity editor due to its simplicity.

```
void Start()
{
    EntityManager entityManager = World.DefaultGameObjectInjectionWorld.EntityManager;
    Entity entity = entityManager.CreateEntity(typeof(LevelComponent));

    entityManager.SetComponentData(entity, new LevelComponent { level = 10 });
}

```

### World
A World owns both an EntityManager and a set of ComponentSystems. By default unity automatically creates a single World when entering Play Mode and populates it with all available ComponentSystem objects in the project. For more information see [link](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/world.html) 

## Components
Components represent the data of your game or program. Entities are identifiers that index your collections of components. Systems provide that behaviour.

A component in ECS is a struct with one of the following "marker interfaces":

1) **IComponentData** — use for general purpose and chunk components.
2) IBufferElementData — use for associating dynamic buffers with an entity.
3) ISharedComponentData — use to categorize or group entities by value within an archetype.
4) ISystemStateComponentData — use for associating system-specific state with an entity and for detecting when individual entities are created or destroyed. See System State Components.
5) ISharedSystemStateComponentData — a combination of shared and system state data.

The EntityManager organizes unique combinations of components appearing on your entities into **Archetypes**. It stores the components of all entities with the same archetype together in blocks of memory called **Chunks**.

### General Purpose Components
ComponentData in Unity is a struct that contains only variables, the instance data for an entity. Unity ECS provides an interface called IComponentData that you can implement in your code.

#### IComponentData
IComponentData is a pure ECS-style component, meaning that it defines no behavior, only data. IComponentData is a struct rather than a class, meaning that it is copied by value instead of by reference by default. 

IComponentData structs may not contain references to managed objects. Since the all ComponentData lives in simple non-garbage-collected tracked chunk memory. For more information on IComponentData see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/component_data.html)

```
using Unity.Entities;

public struct LevelComponent:IComponentData
{
    public float level;
}

```

### Shared Component Data
Shared components are a special kind of data component that you can use to subdivide entities based on the specific values in the shared component (in addition to their archetype).

When you add a shared component to an entity, the EntityManager places all entities with the same shared data values into the same Chunk. Shared components allow your systems to process like entities together.

**Note: Over using shared components can lead to poor Chunk utilization since it involves a combinatorial expansion of the number of memory Chunks required based on archetype and every unique value of each shared component field. Avoid adding unnecessary fields to a shared component Use the Entity Debugger to view the current Chunk utilization.**

IComponentData is generally appropriate for data that varies between entities.
ISharedComponentData is appropriate when many entities share something in common. For example in the Boid demo we instantiate many entities from the same Prefab and thus the RenderMesh between many Boid entities is exactly the same.

The great thing about ISharedComponentData is that there is literally zero memory cost on a per entity basis. For more information on ISharedComponentData see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/ecs_components.html)

```
[System.Serializable]
public struct RenderMesh : ISharedComponentData
{
    public Mesh                 mesh;
    public Material             material;

    public ShadowCastingMode    castShadows;
    public bool                 receiveShadows;
}
```


### System State Components
[To-do](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/system_state_components.html)

## How to Debug in ECS
Use the **Systems** and **Entities Hierarchy** present in **"Window > Entities"**.


## Create Systems
```
using UnityEngine;
using Unity.Entities;

public partial class LevelUpSystem : SystemBase
{
    protected override void OnUpdate()
    {
        float deltaTime = World.Time.DeltaTime;

        Entities.ForEach((ref LevelComponent levelComponent) =>
        {
            levelComponent.level += 1f * deltaTime;
            Debug.Log(levelComponent.level);
        }).ScheduleParallel();
    }
}
```
