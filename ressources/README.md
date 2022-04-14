# secretmanagementinterface package 

# Background
For an easier intergration of the [secretmanagerinterface](https://hackmd.io/EtJSEnxjTVOOvRJdWGJlYw?view) into thingies who need it.

The basis for this is the general style to implement [Thingy Interfaces](https://hackmd.io/SEDcVeUPTliQfjcgG7NT_g?view).

The conclusion was to use an ESM package exporting the client-interface as `sci`
the express routes as `routes` and the handlers as `handlers`.  

The `sci` and the `routes` are a direct result of the SCI definition.
As well are the `handlers` stubs. However the `handlers` content is more specific to the actual functionality. We may still assume that also the specific content of the `handlers` functions is stable/evolving with the SCI and the Service.

Thus we just abstracted away the specific service code which might be arbitrarily implemented. This is the service which needs to be passed to the `handlers` via `handlers.setService(serviceToSet)` and should implement the expected API.

# Usage
Requirements
------------
- nodejs >= 14
- ESM importability

Installation
------------
Current git version:
```
npm install git+https://github.com/JhonnyJason/secretmanagementinterface-package-output.git
```
Npm Registry:
```
npm install secretmanagementinterface
```


## Client Side
This one is straight forward as the direct function to be called is exposed in the `sci` export.

```coffee
import {sci} from "secretmanagementinterface"

result = await sci.addNodeId(sciURL, publicKey, timestamp, signature)

```

## Server Side
Here it is worth to consider the [thingy-sci-base](https://www.npmjs.com/package/thingy-sci-base) package. As this already abstracts away the express and systemd socket handling code.

```coffee
import * as sciBase from "thingy-sci-base"
import { routes, handlers } from "secretmanagementinterface"
import * as service from "./ourSpecificImplementation.js"

handlers.setService(service)
sciBase.prepareAndExpose(service.authenticateRequest, routes)

```

Current Functionality
---------------------
- The `routes` exported functions are what express expects.
- The `routes` expect to be called with a parsed JSON for `req.body`.
- The `routes` will call the specific handler functions.
- The `routes` will send back the JSON how the handlers respond.
- The `routes` will catch any exceptions and respond with an error in that case.
- The `handlers` will call the appropriate `service` function.
- The `handlers` will assert the returned response is valid.
- The `service` functions may be async.

## Expected Service API

```coffee
export  addNodeId = (publicKeyHex) -> return

export removeNodeId = (publicKeyHex) -> return

export getEncryptedSecretSpace(publicKeyHex) -> {referencePointHex, encryptedContentHex}
        
export getSecret = (publicKeyHex, secretId) -> {referencePointHex, encryptedContentHex}

export setSecret = (publicKeyHex, secretId, secret) -> return

export deleteSecret = (publicKeyHex, secretId) -> return

export addSubSpaceFor = (publicKeyHex, fromId) -> return

export removeSubSpaceFor = (publicKeyHex, fromId) -> return

export shareSecretTo = (publicKeyHex, shareToIdHex, secretId, secret) -> return

export deleteSharedSecret = (sharedToIdHex, publicKeyHex, secretId) -> return

export addNotificationHook = (publicKeyHex, type, specific) -> return

export generateAuthCodeFor = (publicKeyHex) -> {authCode}

export addFriendServer = (serverURL, serverNodeIdHex) -> return

export getSignedNodeId = () -> {publicKey, timestamp, signature}
```

---

# Further steps

- fix bugs
- improve flexibility and convenience
- ...


All sorts of inputs are welcome, thanks!

---

# License

## The Unlicense JhonnyJason style

- Information has no ownership.
- Information only has memory to reside in and relations to be meaningful.
- Information cannot be stolen. Only shared or destroyed.

And you wish it has been shared before it is destroyed.

The one claiming copyright or intellectual property either is really evil or probably has some insecurity issues which makes him blind to the fact that he also just connected information which was freely available to him.

The value is not in him who "created" the information the value is what is being done with the information.
So the restriction and friction of the informations' usage is exclusively reducing value overall.

The only preceived "value" gained due to restriction is actually very similar to the concept of blackmail (power gradient, control and dependency).

The real problems to solve are all in the "reward/credit" system and not the information distribution. Too much value is wasted because of not solving the right problem.

I can only contribute in that way - none of the information is "mine" everything I "learned" I actually also copied.
I only connect things to have something I feel is missing and share what I consider useful. So please use it without any second thought and please also share whatever could be useful for others. 

I also could give credits to all my sources - instead I use the freedom and moment of creativity which lives therein to declare my opinion on the situation. 

*Unity through Intelligence.*

We cannot subordinate us to the suboptimal dynamic we are spawned in, just because power is actually driving all things around us.
In the end a distributed network of intelligence where all information is transparently shared in the way that everyone has direct access to what he needs right now is more powerful than any brute power lever.

The same for our programs as for us.

It also is peaceful, helpful, friendly - decent. How it should be, because it's the most optimal solution for us human beings to learn, to connect to develop and evolve - not being excluded, let hanging and destroy oneself or others.

If we really manage to build an real AI which is far superior to us it will unify with this network of intelligence.
We never have to fear superior intelligence, because it's just the better engine connecting information to be most understandable/usable for the other part of the intelligence network.

The only thing to fear is a disconnected unit without a sufficient network of intelligence on its own, filled with fear, hate or hunger while being very powerful. That unit needs to learn and connect to develop and evolve then.

We can always just give information and hints :-) The unit needs to learn by and connect itself.

Have a nice day! :D