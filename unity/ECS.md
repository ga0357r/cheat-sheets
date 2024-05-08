# Entity Component System(ECS)
ECS for Unity (Entity Component System) is a data-oriented framework compatible with GameObjects, enabling seasoned Unity creators to build more ambitious games thanks to an unprecedented level of control and determinism. 

See Physics 2D for 2D physics. [documentation](https://docs.unity3d.com/Packages/com.unity.2d.entities@0.22/manual/physics-example.html)
[To-Do-Information 1](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/index.html). [To-Do-Information 2](https://docs.unity3d.com/Packages/com.unity.entities@1.2/manual/index.html).


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
The purpose of **SystemStateComponentData** is to allow you to track resources internal to a system and have the opportunity to appropriately create and destroy those resources as needed without relying on individual callbacks. 

**SystemStateComponentData** and **SystemStateSharedComponentData** are exactly like **ComponentData** and **SharedComponentData**, respectively, except in one important respect:

1. **SystemStateComponentData** is not deleted when an entity is destroyed. DestroyEntity is shorthand for
a. Find all components which reference this particular entity ID.
b. Delete those components.
c. Recycle the entity id for reuse.

However, if **SystemStateComponentData** is present, it is not removed. This gives a system the opportunity to cleanup any resources or state associated with an entity ID. The entity ID will only be reused once all SystemStateComponentData has been removed.

For more information on System State Components see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/system_state_components.html)

### Dynamic Buffers
A **DynamicBuffer** is a type of component data that allows a variable-sized, "stretchy" buffer to be associated with an entity. It behaves as a component type that carries an internal capacity of a certain number of elements, but can allocate a heap memory block if the internal capacity is exhausted.

Memory management is fully automatic when using this approach. Memory associated with **DynamicBuffers** is managed by the EntityManager so that when a **DynamicBuffer** component is removed, any associated heap memory is automatically freed as well.

**DynamicBuffers** supersede fixed array support which has been removed.

For more information on Dynamic Buffers see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/dynamic_buffers.html)

### Chunk components
Use chunk components to associate data with a specific chunk. The main differences between working with chunk components and general-purpose components is that you use different functions to add, set, and remove them. Chunk components also have their own ComponentType functions for use in defining entity archetypes and queries.

For more information on Chunk components see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/ecs_chunk_component.html)

## Systems
A **System**, the S in ECS, provides the logic that transforms the component data from its current state to its next state — for example, a system might update the positions of all moving entities by their velocity times the time interval since the previous update.

Unity ECS provides a number of different kinds of systems. The main systems that you can implement to transform your entity data are the **ComponentSystem** and the **JobComponentSystem**. 

**In a Job Component System, you typically schedule Jobs in the OnUpdate() function.**  In general, Job Component Systems provide the best performance since they take advantage of multiple CPU cores. Performance can be improved even more when your Jobs are compiled by the Burst compiler.

Systems provide event-style callback functions, such as **OnCreate()** and **OnUpdate()** that you can implement to run code at the correct time in a system's life cycle. 

### System Event Functions
1. OnCreate() -- called when the system is created.
2. OnStartRunning() -- before the first OnUpdate and whenever the system resumes running.
3. OnUpdate() -- every frame as long as the system has work to do (see ShouldRunSystem()) and the system is Enabled. Note that the OnUpdate function is defined in the subclasses of ComponentSystemBase; each type of system class can define its own update behavior.
4. OnStopRunning() -- whenever the system stops updating because it finds no entities matching its queries. Also called before OnDestroy.
5. OnDestroy() -- when the system is destroyed.

All of these functions are executed on the main thread. Note that you can schedule Jobs from the OnUpdate(JobHandle) function of a JobComponentSystem to perform work on background threads.


### Component Systems
A ComponentSystem in Unity (also known as a system in standard ECS terms) performs operations on entities. A ComponentSystem cannot contain instance data. To put this in terms of the old Unity system, this is somewhat similar to an old Component class, but one that only contains methods. One ComponentSystem is responsible for updating all entities with a matching set of components (that is defined within a struct called a EntityQuery).

Unity ECS provides an abstract class called ComponentSystem that you can extend in your code.

For more information on Component Systems see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/component_system.html)


### Job Component Systems
For more information on Job Component Systems see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/job_component_system.html)

#### Automatic job dependency management
Managing dependencies is hard. This is why in JobComponentSystem we are doing it automatically for you. The rules are simple: jobs from different systems can read from IComponentData of the same type in parallel. If one of the jobs is writing to the data then they can't run in parallel and will be scheduled with a dependency between the jobs.

```
public class RotationSpeedSystem : JobComponentSystem
{
    [BurstCompile]
    struct RotationSpeedRotation : IJobForEach<Rotation, RotationSpeed>
    {
        public float dt;

        public void Execute(ref Rotation rotation, [ReadOnly]ref RotationSpeed speed)
        {
            rotation.value = math.mul(math.normalize(rotation.value), quaternion.axisAngle(math.up(), speed.speed * dt));
        }
    }

    // Any previously scheduled jobs reading/writing from Rotation or writing to RotationSpeed 
    // will automatically be included in the inputDeps dependency.
    protected override JobHandle OnUpdate(JobHandle inputDeps)
    {
        var job = new RotationSpeedRotation() { dt = Time.deltaTime };
        return job.Schedule(this, inputDeps);
    } 
}

```

#### How does this work?
All jobs and thus systems declare what ComponentTypes they read or write to. As a result when a JobComponentSystem returns a JobHandle it is automatically registered with the EntityManager and all the types including the information about if it is reading or writing.

Thus if a system writes to component A, and another system later on reads from component A, then the JobComponentSystem looks through the list of types it is reading from and thus passes you a dependency against the job from the first system.

JobComponentSystem simply chains jobs as dependencies where needed and thus causes no stalls on the main thread. But what happens if a non-job ComponentSystem accesses the same data? Because all access is declared, the ComponentSystem automatically completes all jobs running against component types that the system uses before invoking OnUpdate.

#### Dependency management is conservative & deterministic
Dependency management is conservative. ComponentSystem simply tracks all EntityQueryobjects ever used and stores which types are being written or read based on that.

Also when scheduling multiple jobs in a single system, dependencies must be passed to all jobs even though different jobs may need less dependencies. If that proves to be a performance issue the best solution is to split a system into two.

The dependency management approach is conservative. It allows for deterministic and correct behaviour while providing a very simple API.

#### Sync points
All structural changes have hard sync points. CreateEntity, Instantiate, Destroy, AddComponent, RemoveComponent, SetSharedComponentData all have a hard sync point. Meaning all jobs scheduled through JobComponentSystem will be completed before creating the entity, for example. This happens automatically. So for instance: calling EntityManager.CreateEntity in the middle of the frame might result in a large stall waiting for all previously scheduled jobs in the World to complete.

See [EntityCommandBuffer](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/entity_command_buffer.html) for more on avoiding sync points when creating entities during game play.


#### Multiple Worlds
Every World has its own EntityManager and thus a separate set of JobHandle dependency management. A hard sync point in one world will not affect the other World. As a result, for streaming and procedural generation scenarios, it is useful to create entities in one World and then move them to another World in one transaction at the beginning of the frame.



### Entity Command Buffer
For more information on Entity Command Buffer, see the [documentation](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/entity_command_buffer.html)

#### Entity Command Buffer Systems

#### Using EntityCommandBuffers from ParallelFor jobs




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
