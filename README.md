# solana-elasticsearch-plugin

A plugin consumes and index output from Solana Geyser framework plugins for real-time indexing for OLTP/OLAP applications.

## Problem Today

**TL;DR**
If you believe your smart device or phone will have a nuclear plant powering it, DO NOT READ FURTHER.
You want the front-end to process events ~30MB+/sec from a RPC node, or performing on-the-fly filtering? REALLY?
That's the story. We want something more light-weight and legit. Let's go hybrid.

### Context
Listeners like web socket or consuming raw data in the front-end may not be suitable for some applications.
It just does not make sense to consume a lot of resource in the front-end, draining smart devices or laptop batteris. 
This do all the heavy lifting in the back-end and provide a light-weight solution for the front-end without compromising integrity and security.
Front-end can always use the public key to validate the source data. Consider the backend some kind of Oracle.

There are several Solana Geyser framework plugins out there that assist RPC nodes and interfaces (web sockets) on real-time streaming and monitoring, e.g. if you are from the Ethereum world, the following example should look familiar,

```
event someTxForNFT(
    address sender,
    uint256 tokenId
);
```

```
emit someTxForNFT(msg.sender, someTxId);
```

or if you are integrating using `web3.js` e.g. [solana-web3.js](https://github.com/solana-labs/solana-web3.js), you may see something like this in the example code [connection-subscriptions.test.ts](https://github.com/solana-labs/solana-web3.js/blob/master/test/connection-subscriptions.test.ts)

```js
setupAlternateListener(callback: AccountChangeCallback): number {
    return connection.onAccountChange(
      new PublicKey('C2jDL4pcwpE2pP5EryTGn842JJUJTcurPGZUquQjySxK'),
      callback,
    );
  }
```

with an user-defined `listener` and `callback` function in a front-end library.

If you are a front-end developer, and trying to run all the `async` functions to listen and poll from a web socket,
that is just insane. Considering processing ~30MB+/s events emitting from a validator node. REALLY!!!!
That user experience sucks.
