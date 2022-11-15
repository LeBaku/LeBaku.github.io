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
        void onColision(ComponentR thisComp, ComponentR colidComp, EntityType colideType); 

    private:
        std::vector<std::shared_ptr<IComponent>>& _siblings;
};
```
You now have acces at ```onDeath()``` ```onRightScreen()```  ```onLeftScreen()``` ```onColision()``` 

<br /><br />

# Components

## AnimComponent

Constructor
```
AnimComponent()
```

Set if you want to animate your sprite, ```True``` by default
```
void setAnime(bool isAnimated)
bool getAnime()
```

Set the idle Animation <br />
isLoop ```False``` make the animation one time
```
void setIdlAnim(sf::IntRect rect, int nbFrame, float frequency, bool isLoop)
```

Allows to setup a specific animation for any key <br />
isOnce on ```True``` make the animation one time if the key is pressed once
isMaintain on ```True``` make the animation one time and stay on the last animation frame if the player hold the key
```
void setKeysAnim(sf::Keyboard::Key key, sf::IntRect rect, int nbFrame, float frequency, bool isOnce, bool isMaintain)
```


## ColisionComponent

EntityType can be ```ALLY``` ```NEUTRAL``` ```ENEMY``` <br />
Without any precision the type is set to ```NEUTRAL```
```
ColisionComponent()
ColisionComponent(EntityType type)
```

```
void setEntityType(EntityType type)
EntityType getEntityType()
```

    
## GravityComponent

Adds gravity to an Entity
```
GravityComponent()
```
    
## MoveComponent

Handles movement for an Entity 
```
MoveComponent()
```

Set and get the direction of the moves
```
void setMovX(float movX)
void setMovY(float movY)
float getMovX()
float getMovY()
```
    
## PositionComponent

Needed for an Entity to have a position <br />
Without any precision ```x``` and ```y``` are set to ```0```;
```
PositionComponent()
PositionComponent(float x, float y)
```

Set and get the position
```
float getX()
float getY()
void setPos(float x, float y)
```
    
## SceneComponent

Without any precision the idScene is set to -1, which mean the entity is handle everywhere in the game 
```
SceneComponent()
SceneComponent(int idScene)
```

Set and get and idScene
```
void setIdScene(int idScene)
int getIdScene()
```

## ShootComponent

Allows an Entity to generate shoot Entities
```
ShootComponent(std::string path)
```

Get the entity path
```
std::string getPath()
```

Set and get the shootPerSecond
```
void setShootPerSecond(float shootPerSec)
float getShootPerSecond()
```

Set and get the size of the shoot
```
void setSize(float size)
float getSize()
```

    
## SoundComponent

```
SoundComponent()
```

Set the sound id a certain key is pressed
```
void setSoundKey(sf::Keyboard::Key key, std::string path)
```

Set the sound if the entity is deleted
```
void setSoundDeath(std::string path)
```

Seth the sound if the entity is shooting
```
void setSoundShoot(std::string path)
```

Play any sounds (this is handle by the system but the person can use it anyway)
```
void playSoundKeyboard(sf::Keyboard::Key key)
void playSoundDeath()
void playSoundShoot()
```
    
## TextureSpriteComp

```
TextureSpriteComp(std::string path)
```

Set and get the TextureRect
```
void setTextureRect(sf::IntRect newIntRect)
sf::IntRect getTextRect()
```

Set and get the scale
```
void setScale(float scX, float scY)
sf::Vector2f getScale()
```

Get the real size of the sprite (with scale)
```
sf::Vector2f getRealSize()
```

Get the path of the texture
``` 
std::string getPath()
```

Get the SFML sprite
```
sf::Sprite& getSprite()
```

## VectorComponent

Move the sprite with a vector
```
VectorComponent(float vecX, float vecY)
```

Get and set vector
```
float getVectorX()
float getVectorY()
void setVectorX(float x)
void setVectorY(float y)
```
    
## VelocityComponent

Velocity of the sprite
```
VelocityComponent(std::vector<std::shared_ptr<IComponent>>& sibling, EntityManager& admin)
VelocityComponent(std::vector<std::shared_ptr<IComponent>>& sibling, EntityManager& admin, float velX, float velY)
```

Get and set the velocity
```
float getVelocityX()
float getVelocityY()
void setVelocityX(float x)
void setVelocityY(float y)
```
    

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
