# ECS

The Entity Component System makes it easier to create and compute a game. Current available components and systems :
<ul>
    <li>Position (Manage an entity's position)</li>
    <li>Sprite (Allow to draw, set textures)</li>
    <li>Animation (SpriteSheet animation)</li>
    <li>Colision (allow entity to collide with another)</li>
    <li>Gravity (Apply gravity)</li>
    <li>Move (Allow entity to move on player inputs)</li>
    <li>Shoot (Allo entity to shoot on player input)</li>
    <li>Vector (Give direction)</li>
    <li>Velocity (Give speed)</li>
</ul>
To use the ECS you first need to declare <b>Manager</b> which will handle all entities.<br>
You can then add entities using <b>manager.AddEntity()</b> and you attach components to these entities using <b>entity.addComponent<ComponentYouWantToAdd>()</b>.
<ul>
    <li>To create new components you need the new component to inherit from Component.</li>
    <li>To create new systems you need the new system to inherit from System.</li>
</ul>

# ECS Diagram

![](../media/Diagram_ECS.png)

<br /><br />

# Add your own Event

You can custom your own event with inheritance of EventComponent

![](../media/myEvent.png)

<br />

Example:
```
#pragma once
#include "EntityManager.hpp"

class MyEvent : public EventComponent {
    public:
        MyEvent(std::vector<std::shared_ptr<IComponent>>& sibling) : _siblings(sibling) {}
        std::vector<std::shared_ptr<IComponent>>& getSiblings() {
            return _siblings;
        }
        void onDeath(EntityManager& admin);
        void onRightScreen(EntityManager& admin);
        void onLeftScreen(EntityManager& admin);

    private:
        std::vector<std::shared_ptr<IComponent>>& _siblings;
};
```
You now have acces at ```onDeath()``` ```onRightScreen()```  ```onLeftScreen()``` 

<br /><br />

# Examples

Simple example of a single sprite moovable by the player

```
int main()
{
    EntityManager admin;

    EntityR ship = admin.addEntity();
    ComponentR textShip = ship->addComponent<TextureSpriteComp>("./sprites/ship_34_15.png");
    ship->addComponent<PositionComponent>(100, 200);
    ship->addComponent<MoveComponent>();
    ship->addComponent<VelocityComponent>(10, 7);

    textShip->setFrameSize(33, 15);
    textShip->setFrame(2);
    textShip->setScale(2, 2);

    while (admin.isOpen()) {
        admin.update();
    }
    return 0;
}
```