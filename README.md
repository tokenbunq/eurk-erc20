## Contracts

The implementation uses 2 separate contracts - a proxy contract
(`FiatTokenProxy.sol`) and an implementation contract (`FiatToken.sol`). This
allows upgrading the contract, as a new implementation contact can be deployed
and the Proxy updated to point to it.

### FiatToken

The FiatToken offers a number of capabilities, which briefly are described
below. There are more [detailed design docs](./doc/tokendesign.md) in the `doc`
folder.

### ERC20 compatible

The FiatToken implements the ERC20 interface.

### Pausable

The entire contract can be frozen, in case a serious bug is found or there is a
serious key compromise. No transfers can take place while the contract is
paused. Access to the pause functionality is controlled by the `pauser` address.

### Upgradable

A new implementation contract can be deployed, and the proxy contract will
forward calls to the new contract. Access to the upgrade functionality is
guarded by a `proxyOwner` address. Only the `proxyOwner` address can change the
`proxyOwner` address.

### Blacklist

The contract can blacklist certain addresses which will prevent those addresses
from transferring or receiving tokens. Access to the blacklist functionality is
controlled by the `blacklister` address.

### Minting/Burning

Tokens can be minted or burned on demand. The contract supports having multiple
minters simultaneously. There is a `masterMinter` address which controls the
list of minters and how much each is allowed to mint. The mint allowance is
similar to the ERC20 allowance - as each minter mints new tokens their allowance
decreases. When it gets too low they will need the allowance increased again by
the `masterMinter`.

### Ownable

The contract has an Owner, who can change the `owner`, `pauser`, `blacklister`,
or `masterMinter` addresses. The `owner` can not change the `proxyOwner`
address.
