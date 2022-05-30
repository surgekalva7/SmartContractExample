# SmartContractExample

@author: Srujan Kalva
DESCRIPTION: The intent of the project is to get a better understanding of blockchain and smart contracts.

Trading Ethereum:
    Accounts:
        Multiple Accounts - Each account has a different private key.
            Private key(digital signature - world cannot see) uses an algorithm to be converted into a public key(world can see)
            NEVER SHARE PRIVATE KEY
            Private Key -> Public Key -> Address
        Recovery - All the accounts share the same recovery password.
    Networks:
        Test Network - A common test network to get a limited amount of fake ethereum is the rinkeby testnetwork faucet. A faucet is a website that is able to give a limited amount of these ethereum based on the account(0x00...).
        Mainnet - The network that should be used when actually using real Ethereum.
    Gas:
        the cost to perform a transaction on the network, the more busy the network is, the more expensive gas is going to be.

Blockchain Demo:
    - Ethereum uses a Keccak256 hash algorithm.
    - Block #, Nonce, Data, Prev -> Algorithm -> Hash
    - Block - A list of transactions mined together.
    - Nonce - A "number used once" to find the "solution" to the blockchain problem.
    - Mining - the solution to the blockchain problem, which was to find a nonce that would result in a hash that starts with four zereos in the example.
    So when blocks are paired next to one another:
        Block(1):
            Nonce - 11316
            Data - hello
            Prev - 0x0...0000
            Hash - 0x0...1563
        Block(2):
            Nonce - 18432
            Data - hola
            Prev - 0x0...1563 -> matches the previous block
            Hash - 0x0...1738
    However if you change the data of block 1:
        Block(1):
            Nonce - 25910
            Data - namaste
            Prev - 0x0...0000
            Hash - 0x0...8032
        Block(2):
            Nonce - 12131
            Data - hola
            Prev - 0x0...1563 -> No longer matches the previous block and detects a change and nonces won't work.
            Hash - 0x0...1738
    - Having a decentralized database allows for less manipulation of the blocks because if on one peers network something is tampered with, and not on the others then it is clear to see that it was being messed with.
    - Node - a single instance of a decentralized network(the peers).

Consensus:
    - the mechanism udes to agree on the state of a blockchain
    Chain Selection - How to determine what is the true blockchain.
        
    Sybil Resistance - using pseudonymous identities and uses them to gain a disproportionately large influence.
        Proof of Work - 
        Proof of Stake