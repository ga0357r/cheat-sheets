# Entity Component System(ECS)
ECS for Unity (Entity Component System) is a data-oriented framework compatible with GameObjects, enabling seasoned Unity creators to build more ambitious games thanks to an unprecedented level of control and determinism. [To-Do-Information 1](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/index.html). [To-Do-Information 2](https://docs.unity3d.com/Packages/com.unity.entities@1.2/manual/index.html)

The Entity Component System (ECS) is the core of the Unity Data-Oriented Tech Stack. ECS has three principal parts:
1) Entities -  The entities, or things, that populate your game or program.

2) Components - The data associated with your entities, but organized by the data itself rather than by entity. (This difference in organization is one of the key differences between an object-oriented and a data-oriented design.)

3) Systems -  The logic that transforms the component data from its current state to its next state— for example, a system might update the positions of all moving entities by their velocity times the time interval since the previous frame.

## Entities
**Entities represent the individual "things" in you game or program**. An entity has neither behavior nor data; instead, it identifies which pieces of data belong together. Systems provide the behavior. Components store the data. [To-Do-Information 1](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/ecs_entities.html)

## How to Debug in ECS
Use the **Systems** and **Entities Hierarchy** present in **"Window > Entities"**.

## Create Components
```
using Unity.Entities;

public struct LevelComponent:IComponentData
{
    public float level;
}

```

## Create Entities
```
void Start()
{
    EntityManager entityManager = World.DefaultGameObjectInjectionWorld.EntityManager;
    Entity entity = entityManager.CreateEntity(typeof(LevelComponent));

    entityManager.SetComponentData(entity, new LevelComponent { level = 10 });
}

```

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
