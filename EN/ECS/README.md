# ECS

The Entity Component System makes it easier to create and compute a game. Current available components and systems :
<ul>
    <li>Position (Manage an entity's position)</li>
    <li>Sprite (Allow to draw, set textures)</li>
</ul>
To use the ECS you first need to declare <b>Manager</b> which will handle all entities.<br>
You can then add entities using <b>manager.AddEntity()</b> and you attach components to these entities using <b>entity.addComponent<ComponentYouWantToAdd>()</b>.
<ul>
    <li>To create new components you need the new component to inherit from Component.</li>
    <li>To create new systems you need the new system to inherit from System.</li>
</ul>


