# UTXO-based Token Protocol: Fungible
UTXO-based Token Protocol (UTP) aims to provide a unified standard for tokens on Bitcoin SV, which allows maximal interoperability among various token providers and is thus implemention-agonostic.

## Token UTXOs
UTP assumes tokens are stored inside UTXOs, spendable or not. Each token UTXO contains the following information.
- **Token owner**: the owner of tokens within this UTXO. It can be a public key or an address. Tokens in a UTXO are transferred by signing using the private key corresponding to the public key, just like normal bitcoin transfers.
- **Token amount**: quantities of tokens within this UTXO. It can be in the satoshi amount part of the UTXO or the script part, typically embedded using *OP_RETURN* or *OP_PUSHDATA + OP_DROP*.
- **Token rule** [*optional*]: rule for token operations such as transfer and burn. In case the rule is enforced at consesuns layer by miners, it is part of the script of a token UTXO.

## Token APIs
The following interfaces shall be implemented by token providers at upper layer to conform to the UTP standard. They are expressed in the form of [sCrypt](https://scryptdoc.readthedocs.io) code. Note that the final implementation by each token provider will be semantically equivalent, but syntactically different.

### Required APIs
The following APIs are required:
- `function balanceOf(PubKey owner) returns int`
    
    Returns the token balance of `owner`.
- `function transfer(PubKey from, PubKey to, int amount) returns bool`

    Transfer `amount` tokens from `from` to `to`. Returns success or not.
- `function totalSupply() returns int`
    
    Returns total supply, which equates `initial supply + minted tokens - burnt tokens`.

### Optional APIs
In additional to required APIs, other APIs can be offered by each implementation. For example,
- `function mint(PubKey to, int amount) returns bool`

    Mint `amount` new tokens to `to`. Returns success or not.
- `function burn(PubKey owner, int amount) returns bool`

    Burn `amount` tokens belonging to `owner`. This could be done either by make `owner`'s certain token UTXOs unspendable or turn them back to spendable bitcoin UTXOs. Returns success or not.

## Token Metadata
Token metadata are embedded in one of the outputs of the token issuance transaction. They can be, but are not required to be, in each token UTXO since they are the same for a given token. They can be in an *OP_RETURN* data-carrier output or appended at the end of a spendable output using *OP_RETURN* or *OP_PUSHDATA + OP_DROP*. They are encoded using *OP_PUSHDATA* in the following order:
1. **Name**: string, name of the token, e.g., “Alice Token”
2. **Symbol**: string, symbol of the token, e.g., “AT”
3. **InitialSupply**: int, initial supply of tokens at issuance
4. **ID [optional]**: string, unique token ID. It can be implicit and just be the txid of the issuance transaction, optionally plus output index.
5. **Issuer**: string, token issuer.
6. **Mintable**: bool, whether new tokens can be minted after the initial issurace

## Implementation
[Example implementation](https://medium.com/@xiaohuiliu/utxo-based-layer-1-tokens-on-bitcoin-sv-f5e86a74c1e1).

## Copyright
[Open BSV License](https://github.com/scrypt-sv/utp/blob/master/LICENSE)
