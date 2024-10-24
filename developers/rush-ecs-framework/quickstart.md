---
description: This page lists the steps on how to setup Rush.
---

# Quickstart

{% hint style="warning" %}
🚧 Rush is under heavy development. Expect bugs, crashes, breaking changes, and partial experiences so we can ship fast, break things, and iterate to bring you **the best onchain game development experience**. \
\
This page is <mark style="color:orange;">**Updated Daily**</mark>.
{% endhint %}

## Prerequisite: Install the Rust and Solana toolchain

* [Install the Rust toolchain](https://www.rust-lang.org/tools/install)
* [Install the Solana toolchain](https://docs.solanalabs.com/cli/install)



## Install Rush CLI with Cargo

```bash
cargo install rush_ecs_cli
```

Check for successful installation by running

```bash
rush
```

If successfully installed it should print out the following

```bash
Rapid and Declarative development framework for Fully Onchain Games (FOCG) and Autono
mous Worlds (AW) by SonicSVM.

Usage: rush [COMMAND]

Commands:
  new     Create a new Rush project.
  deploy  Deploy current Rush project
  view    Create a new Rush project.
  help    Print this message or the help of the given subcommand(s)

Options:
  -h, --help  Print help

```



## Build Your Game

{% hint style="info" %}
Rush is currently at prerelease and only supports the [Bevy Game Engine](https://bevyengine.org/).
{% endhint %}

Clone the [`rush-quickstart`](https://github.com/mirrorworld-universe/rush-quickstart) repository to study a barebones scaffolding of a simple Player movement game.



## Create a new Rush Workspace

Using the Rush CLI, create a new Rush workspace to generate the **Rush Manifest** and initial **Rush Gaming Blueprint.**

Use the following command

```bash
rush new <NAME>
```

For example

```
rush new my-onchain-game
```

A successful Rush workspace creation will display the following

```bash
⠀[SUCCESS] Rush project created.
```

It will also create a new folder with the following folder structure

```
my-onchain-game/
    Rush.toml
    blueprint/
        world.toml
```

Where `Rush.toml` is your **Rush Manifest** and `world.toml` is your **Rush Gaming Blueprint**.

## Configure Rush Manifest

After creating your Rush workspace, modify the `Rush.toml` and replace the fillers with actual values.

See **References** for **recommended values**.

```toml
[workspace]
name = "my-onchain-game"

[storage]
repository = "solana"

[solana]
store = "<STORAGE_PROGRAM_ADDRESS>"
rpc = "<HTTPS_RPC_URL>"
keypair = "<KEYPAIR_PATH>"

```



## Configure Gaming Blueprint

Next, configure your Gaming Blueprint to define the data you are able to interact with in your Onchain World using the Rush SDKs.

```toml
[world]
name = "My Onchain World"
description = "My Onchain World Description"
entities = ["player"]
regions = ["base"]

[entity]
player = { name = "String", x ="f64", y = "f64"}

[base]
player = [
	{ name = "Player1", x = 0.0, y = 0.0 }
]
```

For example the Gaming Blueprint above defines a 1 Entity `player`, 1 Region `base`, 1 Instance `player` under the Region `base`.



## Deploy Your Onchain World

Now that you're done configuring your Rush Manifest and Rush Gaming Blueprint, it's time to deploy your Onchain World (migration) with the **Rush CLI.**

```bash
rush deploy
```

A successful deployment would yield a similarly looking output below.

```bash
> $ rush deploy                                                            ⬡ 18.19.1 
[SUCCESS] Created world: 8b4ywLN4SCKKNJRECcg9CZ8XR1XWdEGDCv4dY5yeRAS2, Signature: 5Jag
sxDmpmXq86JiLDnRDanovwZGFxvFi4hAUogaHNV3xySYa3hykyuvrKBsZ5xvh5tB84WWGTneHQeT9U71K1Mm
[SUCCESS] Spawned #1: 2hTonMZgSyiPuG7Y4XWSiy7eeh4jTL29826rr5LLpQYb, Signature: 23e1aBj
fQjj7DZDyHn2P3f5N3LhKRwXHtd46ABE62zbBoYaUpiSEuH1aFneFfqxyAhnxu5PzKj5VMta5nR2X9P8Q
```

## Interact with your Onchain World

To interact with your Onchain World, you need to add the `rush_ecs_core` package and `rush_ecs_sdk` package with the command below.

```bash
cargo add rush_ecs_core rush_ecs_sdk
```

Import them into your Bevy game.

```rust
use rush_ecs_core::blueprint::{Component, ComponentValue};
use rush_ecs_sdk::bevy::BevySDK;
```

Instantiate the SDK and use it to interact with your Onchain World.

```rust
fn update(mut player_query: Query<(&mut Transform, &Position), With<Player>>) {
    let mut sdk = BevySDK::new(
        "https://devnet.sonic.game".to_string(),
        "8npxEZiWoi6zcBQ4Pw2e5enC1Av4UhzA2ZtPn1fKeciU",
        "onchain/blueprint",
        "/Users/user/.config/solana/id.json",
    );

    let x_value = sdk
        .get("farm".to_string(), "player".to_string(), 1, "x".to_string())
        .unwrap();
    let y_value = sdk
        .get("farm".to_string(), "player".to_string(), 1, "x".to_string())
        .unwrap();

    let x = x_value.unwrap_float();
    let y = y_value.unwrap_float();

    for (mut transform, position) in &mut player_query {
        transform.translation.x = x as f32;
        transform.translation.y = y as f32;
    }
}

```



See **Reference** to see more available SDK functions.
