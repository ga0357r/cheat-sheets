# Entity Component System(ECS)
ECS for Unity (Entity Component System) is a data-oriented framework compatible with GameObjects, enabling seasoned Unity creators to build more ambitious games thanks to an unprecedented level of control and determinism.

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
