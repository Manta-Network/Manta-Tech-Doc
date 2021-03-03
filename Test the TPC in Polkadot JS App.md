# Test the TPC in Polkadot JS App

1. Start your relay chain or parachain and use the polkadot JS App to join in
2. Click the Develop/Javascript and join in the javascript playground

![Test%20the%20TPC%20in%20Polkadot%20JS%20App%20d32944dc55c543a6a15e68632d07431c/Untitled.png](Test%20the%20TPC%20in%20Polkadot%20JS%20App%20d32944dc55c543a6a15e68632d07431c/Untitled.png)

3.  Use the example called "Make transfer and listen to events" and you can start and listen a transfer. It use the api from pallet "balance".

4.  Add the code to test the time

```jsx
// All code is wrapped within an async closure,
// allowing access to api, hashing, keyring, types, util.
// (async ({ api, hashing, keyring, types, util }) => {
//   ... any user code is executed here ...
// })();

// Make a transfer from Alice to Bob and listen to system events.
// You need to be connected to a development chain for this example to work.
const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY';
const BOB = '5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty';

// Get a random number between 1 and 100000
const randomAmount = Math.floor((Math.random() * 100000) + 1);

// Create a extrinsic, transferring randomAmount units to Bob.
const transfer = api.tx.balances.transfer(BOB, randomAmount);

// Sign and Send the transaction
var beginTime = +new Date();
await transfer.signAndSend(ALICE, ({ events = [], status }) => {
  if (status.isInBlock) {
    console.log('Successful transfer of ' + randomAmount + ' with hash ' + status.asInBlock.toHex());
  } else {
    console.log('Status of transfer: ' + status.type);
  }

  events.forEach(({ phase, event: { data, method, section } }) => {
    console.log(phase.toString() + ' : ' + section + '.' + method + ' ' + data.toString());
  });
  var endTime = +new Date();
  console.log("Transfer Time is: "+(endTime-beginTime)+"ms");
});
```