# Tutorials: How to test pallet on Parachain

1. Add the pallet into runtime/cargo.toml and runtime/src/lib.rs

    little different from add pallet into substrate node, here need a little change when you add a pallet into cargo.toml.

    use nick as a example, usually we add the code like this

    ```rust
    [dependencies]
    #--snip--
    pallet-nicks = { default-features = false, version = '2.0.0' }
    ```

    but when add into parachain, we need add as it

    ```rust
    [dependencies.pallet-nicks]
    default-features = false
    git = 'https://github.com/paritytech/substrate.git'
    branch = 'rococo-v1'
    version = '2.0.0'
    ```

    also, when you add the pallet into lib.rs, also need to change "Trait" into "Config". like this

    ```rust
    impl pallet_nicks::***Config*** for Runtime {
        // The Balances pallet implements the ReservableCurrency trait.
        // https://substrate.dev/rustdocs/v2.0.0/pallet_balances/index.html#implementations-2
        type Currency = pallet_balances::Module<Runtime>;

        // Use the NickReservationFee from the parameter_types block.
        type ReservationFee = NickReservationFee;

        // No action is taken when deposits are forfeited.
        type Slashed = ();

        // Configure the FRAME System Root origin as the Nick pallet admin.
        // https://substrate.dev/rustdocs/v2.0.0/frame_system/enum.RawOrigin.html#variant.Root
        type ForceOrigin = frame_system::EnsureRoot<AccountId>;

        // Use the MinNickLength from the parameter_types block.
        type MinLength = MinNickLength;

        // Use the MaxNickLength from the parameter_types block.
        type MaxLength = MaxNickLength;

        // The ubiquitous event type.
        type Event = Event;
    }
    ```

    also don't forget to add the "std" feature in **cargo.toml** and **construct_runtime**! Macro

    ```rust
    construct_runtime!(
        pub enum Runtime where
            Block = Block,
            NodeBlock = opaque::Block,
            UncheckedExtrinsic = UncheckedExtrinsic
        {
            /* --snip-- */

            /*** Add This Line ***/
            Nicks: pallet_nicks::{Module, Call, Storage, Event<T>},
        }
    );
    [features]
    default = ["std"]
    std = [
        #--snip--
        'pallet-nicks/std',
        #--snip--
    ]

    ```

2. We build and start the parachain showed in cumulus-workshop, and test the pallet with the Polkadot JS Apps UI.

    Here we need change the port combined with parachain.

    We start the parachain with 

```rust
parachain-collator \
  --collator \
  --tmp \
  --parachain-id 200 \
  --port 40333 \
  --**ws-port 9844 \**
  --alice \
  -- \
  --execution wasm \
  --chain <relay chain spec json> \
  --port 30343 \
  --ws-port 9977
```

Change the custom endpoint with ws-port here

![https://github.com/Manta-Network/Manta-Tech-Doc/blob/main/TutorialsPicture/1.png](Tutorials/1.png)

Start the Developer/Extrinsic, select a account and we can test nicks here

![https://github.com/Manta-Network/Manta-Tech-Doc/blob/main/TutorialsPicture/2.png](Tutorials/2.png)
