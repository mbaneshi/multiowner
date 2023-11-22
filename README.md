# Multiowner Wallet
# Multiapproval wallet

This set of contracts provides "N-of-M multisig" functionality: at least N parties out of the predefined set of M _owners_ must approve **Order** to execute it.

Each **Order** may contain an arbitrary number of actions: outcoming messages and updates of parameters. Since the content of the messages is arbitrary **Order** may execute arbitrary high-level interactions on TON: sending TONs, sending/minting jettons, executing administrative duty, etc.

Parameters, such as threshold N, list of _owners_, and others can only be updated by consensus of current N-of-M owners.

Any _owner_ may propose new **Order**. Multiapproval wallet also allows to assign _proposer_ role: _proposer_ may suggest new Orders but can not approve them.

Each **Order** has an expiration date after which it can not be executed.

Each _owner_ may be a wallet, hard-ware wallet, multisign/multiapproval themselves as well as other smart contracts with their own logic.

This Multiapproval wallet was developed keeping in mind [Safe{Wallet}](https://app.safe.global/welcome).

## Guarantees

- Nobody except _proposers_ and _owners_ can initiate creation of new order, nobody except _owners_ can approve new order.
- Change of the _owners_ set invalidates all orders with the old set.
- _Owner_ compromise, in particularly compromise of less than N _owners_, does not hinder to execute orders or to propose new ones (including orders which will remove compromised _owners_ from the owner's list)
- _Proposer_ compromise does not hinder to execution orders or proposition new ones (including orders which will remove the compromised _proposer_ from the proposer's list)
- Logic of multi approval wallet can not changed


## Experimental features
Basic Multiapproval wallet **does not require** experimental features.

By default experimental features are off and can not be activated after the Multiapproval wallet is deployed. Being disabled, experimental features do not interfere with main functionality in any way.

### Module
Module (following Modules design for [Safe Modules](https://docs.safe.global/safe-smart-account/modules)) add custom features to Multiapproval wallet. The module is a smart contract that adds functionality while separating module logic from the Multiapproval walle. A basic Multiapproval wallet **does not require** any modules. Adding and removing a module requires confirmation from the configured threshold number of owners. **Module can provide arbitrary logic, including bypassing security of Multiapproval wallet core, do not add Module to the Multiapproval wallet if you do not build Module yourself**. Security of the Multiapproval wallet with the added Module must be considered in aggregate, aside from the security of the Multiapproval core.

### Guard
The guard is used when there are restrictions on top of the n-out-of-m scheme. Guard checks are executed at the end of the Computation phase: thus after messages and storage updates are prepared, but prior to any messages being sent or storage is updated. Guard has it's own storage which can be used to make history-based checks. Basic Multiapproval wallet **does not require** Guard. Adding and removing a Guard requires confirmation from the configured threshold number of owners. **Guard can provide arbitrary logic, including bypassing security of Multiapproval wallet core, do not add Guard to the Multiapproval wallet if you do not build Guard yourself**. Security of Multiapproval wallet with added Guard must be considered in aggregate, aside from security of Multiapproval core.

Important: Since a Guard has full power to block Order execution, a broken Guard can cause a denial of service for a Multiapproval wallet. Make sure to audit the Guard code and pay attention to recovery mechanisms.




## Project structure

-   `contracts` - source code of all the smart contracts of the project and their dependencies.
-   `wrappers` - wrapper classes (implementing `Contract` from ton-core) for the contracts, including any [de]serialization primitives and compilation functions.
-   `tests` - tests for the contracts.
-   `scripts` - scripts used by the project, mainly the deployment scripts.

## How to use

### Build

`npx blueprint build` or `yarn blueprint build`

### Test

`npx blueprint test` or `yarn blueprint test`

### Deploy or run another script

`npx blueprint run` or `yarn blueprint run`
