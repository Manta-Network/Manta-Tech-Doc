How to run Manta parachain local deployment
============================================

1. Build parachain node, in this current folder, run `cargo build ---release` 
2. Next, we need to build the relay-chain:
   ```
   git clone https://github.com/paritytech/polkadot.git
   # Switch into the Polkadot directory
   cd polkadot
   
   # checkout to the tested commit
   git checkout afb6da

   # Build the Relay Chain Node
   cargo build --release --features=real-overseer

   # Print the help page to ensure the node build correctly
   ./target/release/polkadot --help
   ``` 
 3. download the configuration file:
    https://substrate.dev/cumulus-workshop/shared/chainspecs/rococo-local.json
    
 4. Launch replay chain:
    * launch Alice node
    ```
    ./target/release/polkadot\
    --chain rococo-local.json \
    --tmp \
    --ws-port 9944 \
    --port 30333 \
    --alice
    ```
    * launch Bob node
    ```
    ./target/release/polkadot\
    --chain rococo-local.json \
    --tmp \
    --ws-port 9955 \
    --port 30334 \
    --bob\
    ```
    after launch `Alice` or `Bob`, a peer id of the node will be returned, e.g.:
    ```
    Local node identity is: 12D3KooWG4LBxqznbEyX4Dn9NDE6NY5jTqkf1AgVNQHJ2PJhnpRr
    ```
    
  5. Generate parachain genesis and then generate parachain WASM binary
     ```
     # generate genesis
     ./target/release/parachain-collator export-genesis-state --parachain-id 200 > para-200-gene  
   
     # generate WASM binary
     ./target/release/parachain-collator export-genesis-wasm > para-200-wasm
     ```
  
  6. Start the Collator Node. We can now start the collator node with the following command. Notice that we need to supply the same relay chain spec we used when launching relay chain nodes.
     ```
     parachain-collator \
     --collator \
     --tmp \
     --parachain-id 200 \
     --port 40333 \
     --ws-port 9844 \
     --alice \
     -- \
     --execution wasm \
     --chain <relay chain spec json> \
     --port 30343 \
     --ws-port 9977
     ```
     We give the collator a base path and ports as we did for the relay chain node previously. We also specify the parachain id. Remember to change these collator-specific values if you are executing these instructions a second time for a second parachain. Then we give the embedded relay chain node the relay chain spec we are using. Finally, we give the embedded relay chain node some peer addresses.

