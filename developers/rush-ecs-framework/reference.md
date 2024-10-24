---
description: This page serves as a reference for Rush and its functions.
---

# Reference

{% hint style="warning" %}
🚧 Rush is under heavy development. Expect bugs, crashes, breaking changes, and partial experiences so we can ship fast, break things, and iterate to bring you **the best onchain game development experience**. \
\
This page is <mark style="color:orange;">**Updated Daily**</mark>.
{% endhint %}

To interact with the onchain **Rush Store Program**, Rush has a provided prerelease SDK with the following functions below.

## Rush Manifest

```toml
[workspace]
name = "my-onchain"

[storage]
repository = "solana"

[solana]
store = "<STORAGE_PROGRAM_ADDRESS>"
rpc = "<HTTPS_RPC_URL>"
keypair = "<KEYPAIR_PATH>"

```

The Rush Manifest defines the project configurations to be used for your Rush project.

### Recommended Values

#### Sonic Devnet

`store` = `8npxEZiWoi6zcBQ4Pw2e5enC1Av4UhzA2ZtPn1fKeciU`

`rpc` = [https://devnet.sonic.game](https://devnet.sonic.game)



## Rush Gaming Blueprint: TOML DSL

Supported Types: `i64`, `f64`, `bool`, and `String`

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

The Rush Gaming Blueprint greatly influences how your game interacts with the Onchain World.

In the example above, the Onchain world has a player entity with a `name`,`x`, and `y` component that can be interacted with the Rush SDKs.



## Rush Onchain Data Layout

{% hint style="danger" %}
Rush is currently in prerelease, this Onchain Data Layout is bound to change in the future.
{% endhint %}

### World

```rust
pub struct World {
    /// Identifier for this specific structure
    pub discriminator: [u8; 8],

    /// Description of the world
    pub name: String,
    /// Description of the world
    pub description: String,
    /// Onchain record of what Entity types exist in the world
    pub entities: Vec<Entity>,
    /// Onchain record of what Regions exist in the world
    pub regions: Vec<Region>,
    /// Source of truth for what Instances exist in the world
    pub instances: BTreeMap<Region, BTreeMap<Entity, u64>>,
    /// Determines if the World is already launched and
    /// instances can now be Created, Updated, and Deleted
    /// outside of the CreateWorld (Initialization) Instruction
    pub is_launched: bool,
    /// Overaching authority who has access to state changing
    /// operations
    pub world_authority: Pubkey,

    /// Canonical bump for World
    pub bump: u8,
}

```

### Instance

```rust
pub struct Instance {
    /// Identifier for this specific structure
    pub discriminator: [u8; 8],

    /// Source of truth for what the values of the components are
    pub components: BTreeMap<Component, ComponentValue>,
    /// Nonce to allow multiple Instances
    pub nonce: u64,
    /// Instance authority who has access to state changing
    /// operations in this specific Instance
    pub instance_authority: Pubkey,

    /// Canonical bump for Instances
    pub bump: u8,
}

```



## Bevy SDK

```rust
pub struct BevySDK {
    keypair: Keypair,
    storage: Box<dyn Storage>,
}


impl BevySDK {
    pub fn new(
        rpc_url: String,
        program_id: &str,
        blueprint_path: &str,
        keypair_path: &str,
    ) -> Self {}

    pub fn migrate(&mut self) -> Result<()> {}

    pub fn create(&mut self, region: Region, entity: Entity) -> Result<u64> {}

    pub fn get(
        &mut self,
        region: Region,
        entity: Entity,
        nonce: u64,
        component: Component,
    ) -> Result<ComponentValue> {}

    pub fn set(
        &mut self,
        region: Region,
        entity: Entity,
        nonce: u64,
        component: Component,
        value: ComponentValue,
    ) -> Result<()> {}

    pub fn signin(&self) -> Keypair {}
}

```



