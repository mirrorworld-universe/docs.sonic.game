---
description: This page introduces Rush and its core concepts.
---

# Introduction

{% hint style="warning" %}
🚧 Rush is under heavy development. Expect bugs, crashes, breaking changes, and partial experiences so we can ship fast, break things, and iterate to bring you **the best onchain game development experience**. \
\
This page is <mark style="color:orange;">**Updated Daily**</mark>.
{% endhint %}

Rush is a declarative and rapid Entity-Component-System (ECS) framework completely built with Rust that obsessively pursues a single goal: use already-proven techniques of developer experience abstractions to reduce the complexity of integrating with blockchain technology into already-known developer tooling: Game Engine SDKs and APIs.

<figure><img src="../../.gitbook/assets/rush way.png" alt=""><figcaption><p>Peepo loves the Rush way</p></figcaption></figure>

Rush envisions a future where any game that’s already built or going to be built can easily be turned into a Fully-Onchain Game / Autonomous World simply by using Rush with their favorite Game Development stack.\


## How Rush Works

Typically, a Game Developer uses a Game Engine to create games. With a Game Engine, the complexity of the underlying logic is greatly reduced. Game Developers can focus on their Game Design and Game Mechanics because the Game Engine is designed to take on the complexity.



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdoDbN0yy-Fh4NEsXjoNxtq4gAYkGSLTMwDVlek3Oo3LVMThSWTxMt7qoyjTuiBMaX0nTridE0JXG78t5-VjDRL9mqSqAL_x6kqWPvmxzp0Ti53jy0l7PPErAd8FOIyKyb_EI6uil5LD-MeJOibrh3iuP-p?key=hFrDrzYNUz8HnM_yQC-GsA" alt=""><figcaption><p>Figure 1: Typical Game Developer and Game Engine Relationship</p></figcaption></figure>

On the other hand, Full Onchain Games (FOCG) and Autonomous Worlds (AW) are only possible with a decentralized data store such as the Blockchain because its decentralized nature allows data to persist much more resiliently than if it was stored on a single data repository.\
\
However, this doesn’t come without a cost.



**The Game Developer must now worry about the implementation details of the Blockchain end-to-end tech stack.**

\
This can be solved either by the Game Developer learning it or hiring an already expert both of which cost resources and are often big hurdles for Game Developers to overcome before they consider FOCGs or AWs.

\
**This is why Rush came to be.**\
\
Rush intends to remove all of this complexity by using already-proven effective developer experience abstractions.\


With **Declarative Configuration**, **Entity-Component-System (ECS)**, and **Code Generation** being the main developer experience abstraction strategies used.

\
The Game Developer doesn’t have to learn Blockchain technology’s end-to-end stack.

**The Game Developer just needs to use Rush.**

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdsItB1RMIRyNbyVYRYpBYKqWuE7E06vRlzqKx5MyAfP7AnvjLI70fxnH5AxVIZBQSJ_JyFygLQMQyVobqo8jSieR4PhpRqMh1x89h3wapAZO0gUS_tWPv3L37mg31TYM_zJ1CC9HD7chq5Bolyl5X9PnJY?key=hFrDrzYNUz8HnM_yQC-GsA" alt=""><figcaption><p>Figure 3: Game Developer using Rush to Create an FOCG/AW</p></figcaption></figure>

## The Rush User Journey

To paint a better picture of how Rush works, here’s a typical user journey of a Game Developer using Rush.

1. Build a game with their favorite Game Engine
2. Download the Rush CLI tool from the Sonic repository
3. Create Blueprints of their Game World to be stored onchain with Rush TOML DSL
4. Use Rush CLI to parse Blueprints and generate the following:
   1. Smart Contract Code
   2. SDKs that can interact with the Smart Contract
5. Use the SDKs generated for their favorite Game Engine
6. Use Rush CLI to deploy and manage the onchain program



## The Rush Way

The following principles drive Rush:

1. Declarative - no knowledge of control flow and logic is required to be productive.
2. Rapid - iterating should be easy with lightweight tooling and loosely coupled integration.
3. Entity-Component-System (ECS) - games are reduced to simple and digestible pieces of data.
4. Simple - developer experience is the top priority
5. Product-First - value is accrued by Product-Community fit and not by Complexity and Confusion driving short-term FOMO.
6. Ship faster, break things - the goal is to reduce assumptions and increase facts by iterating with lightning speed even at the expense of things breaking
7. Fun - I mean… why not?\


## Rush Subatomics

Rush as a tool comprises 7 subatomic concepts called the Rush Subatomics.

Much like how neurons, protons, and electrons make up an atom Rush is made up of its World, Entities, Components, Systems, Instances, Blueprints, and Gaming Primitives.\


### World

Onchain data tracking the state of instances in the Game World.

### Entities

Entity is a structure of data that represents a certain Gaming Primitive. For example, a Sheep entity may have an X, Y, Width, Height, and Speed.

### Components

Components determine the data that the Entity holds. For example, a Sheep entity's X or Y position is each a component.

### Systems

Systems take a set of inputs and define the allowed state transitions that can occur onchain with those inputs. For example, a MoveForward system can only allow a Sheep entity to move forward.

### Instances

Instances are actual data in an Entity.\
\
For example, a Sheep entity could be represented like below:\


```
Sheep Entity
X: float
Y: float
W: float
H: float
```

An instance on the other hand instantiates this Entity’s data structure into actual data like below:

```
Sheep Instance
X: 123.0
Y: -10.0
W: 32.0
H: 32.0
```

### Blueprints

Blueprints define the structure of a Game World in Rush DSL format, with TOML being the only one currently supported. It specifies what data should be stored per Entity that is available in the World.

#### Sample Blueprint

```toml
[world]
name = "Your Farmland"
description = "Land of the farm"
entities = ["player"]
regions = ["farm"]

[entity]
player = { x = "f64", y = "f64" }

[farm]
player = [
	{ x = 0.0, y = 0.0 },
]
```



### Gaming Primitives

Gaming Primitives are a certain definition of an entity onchain. For example, a Sheep Gaming Primitive can be represented via the Blueprint.

#### Sample Sheep Gaming Primitive

```toml
[entity.sheep]
x = "f64"
y = "f64"
w = "f64"
h = "f64"
```



## Where is Rush currently?

Rush is currently in **Prerelease** to gather feedback and feature requests directly from the community.
