UTXO-based Token Protocol
UTP aims to provide a uniform standard for tokens on Bitcoin SV, which allows interoperability among various token providers. Tokens are stored inside the script part of a spendable UTXO. At layer 1, each token UTXO’s script consists of four parts: 
token contract code: the business logic of a token contract that encodes rules for token operations.
OP_RETURN: separating code and data part of the contract.
A public key: to control access of the tokens. Tokens in a UTXO are transferred by signing using the private key corresponding to the public key, just like normal bitcoin transfers.
An amount: constituting multiple numbers, whose sum is the number of tokens in this UTXO. Numbers can be negative.
The division of script into code (part 1) and data (part 3 and 4) is a generic pattern to maintain state in bitcoin smart contracts.
Token APIs
The following interfaces shall be implemented by token providers at layer 2 to conform to the UTP standard. They are expressed in the form of sCrypt code. Note that the final implementation by each token provider will be semantically equivalent, but may be syntactically different.
Required APIs
function balanceOf(PubKey owner) returns int
function transfer(Sig ownerSig, PubKey to, int amount) returns bool
function totalSupply() returns int
Total supply = initial supply + minted tokens - burnt tokens
Optional APIs
function mint(Sig issuerSig, PubKey to, int amount) returns bool
function burn(Sig ownerSig, PubKey to) returns bool
Token Metadata
Token metadata are expected to be in one of the outputs of the token issuance transaction. They are not embedded in token UTXOs since they are the same for a given token. They can be in an OP_RETURN data-carrier output or appended at the end of a spendable output using OP_RETURN. They are encoded and placed into OP_RETURN using OP_PUSHDATA in the following order:
Name: string, name of the token, e.g., “Alice Token”
Symbol: string, symbol of the token, e.g., “AT”
InitialSupply: int, initial supply of tokens at issuance
Mintable: bool, whether new tokens can be minted after the initial issurace
Token ID [optional]: string, unique token ID. It can be implicit and just be the txid of the issuance transaction, optionally plus output index.
Implementation
Example implementation.
Copyright
Open BSV License
