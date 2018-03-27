# mew-signer-peer
Client For mew-signer-hs


### Getting Started


### Running the Example:
>Clone the repo:

`git clone <repo address>`

>Install the dependencies:

`npm install`

>Start the server serving the example initiator and receiver:

`npm start`

>Start the signaling server:

`npm start:signal`

>Open two browser tabs/windows:

navigate one to https://localhost:3000/initiator

navigate the other to https://localhost:3000/receiver

_**Note:** You may need to navigate to https://localhost:3001 to accept the self-signed certificate used in the example_

### Launching demo


### Usage

Two Peers are needed with one designated as the Initiator and the other as the Receiver.


```javascript
let mewConnect = new MewConnect(signalStateChange, logger, depends);
```
The constructor takes:
- first argument:  
    - A listener for lifecycle events or null
  ```javascript
        let signalStateChange = function(signal, data){
          if(signal === "codeDisplay"){
              console.log(data); // this is the code that gets entered into the receiver
            };
          };
   ```
    - If null listeners can be attached for specific lifecycle events via ```javascript registerLifeCycleListener```


- second argument:
    - a optional logger or null (to use the default)
- third argument: 
    - a dictionary (object) containing dependencies as they are declared in the scope.
    - ```javascript
          let cryptoFuncs = new MewConnectCrypto(CCrypto.crypto, CCrypto.secp256k1, EthUtilities, BBuffer.Buffer);
          
          let depends = {wrtc: MewRTC,
               cryptoImpl: cryptoFuncs,
                io: io, 
                ethUtils: ""
          };
      ```
        - Note: If running under node (e.g. using webpack or browserfy) this can be omitted as the dependencies will be required via node.js's require during the build process.

#### Initiator

The url of the signaling server is passed to the _initiatorStart_ method on MewConnectInitiator which begins the sequence by connecting to the signaling server and waiting for the signal indicating a receiver peer is ready.
```javascript
let url = "https://localhost:3001";  //Url to the signaling server
mewConnect.initiatorStart(url);
```


#### Receiver

The url of the signaling server and an object containing the key and connection Id from the initiator is passed to the _receiverStart_ method on MewConnect.  This begins the sequence of connecting to the signaling server and then creating the WebRTC connection between the Initiator and Receiver.
- if no initiator peer exists for the Receiver then the connection will fail.

```javascript
let parameters = {
    key: "parth of the connection code before the dash",
    connId: "part of the connection code after the dash"
};
```
_or using the helper on MewConnect_

```javascript
let parameters = MewConnect.parseConnectionDetailString(connectionCode);
```

```javascript
let url = "https://localhost:3001"; //Url to the signaling server
mewConnect.receiverStart(url, parameters);
```
