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
        Proof of Work - Uses competetive validation method to confirm transactions and add new blocks to the blochchain(Lots of Energy) ETH
        Proof of Stake - randomly select miners to validate transactions(Better for the enviroment) ETH 2.0
    Block Confirmations - The number of blocks being added to the block in the longest line of blocks

Attacks:
    Sybil - Numerous accounts created to influence the network
    Longest Chain Rule - The longest chain is essentially the leader and when it adds a new block the others in the network will do the same.

Scalability:
    - When a lot of people want to use the blockchain, the price for the gas will sky rocket, which means gas increases as more people will join.
    Sharding - They will increase the amount of block space by adding new chains to the side of the existing main base chain

Solidity:
    Defining the version:
        pragma solidity <=0.6.0 <0.9.0
    Initialization:
        uint256 fav_num; -> This will result in 0
        uint256 favorite_num = 5;
        bool favorite_bool = true;
        string favorite_string = "String";
        int256 favorite_int = -5;
        address favorite_address = 0x2389028390829038018398290
        bytes32 favorite_bytes = "cat"
    4 Forms of visibility:
        external
        public
        internal
        private
    View Functions:
        We want to view some state of the blockchain
    Pure Functions
        Does not save state
    License Identifier:
        - In order to make the code open source, which most smart contract are, a License Identifier is required
        - (//SPDX-License-Identifier: MIT)

Returns:
    function retrieve() public view returns (uint256){
        return number;
    }
    - It is a view function since we are not changing the state


Structs(Objects):
    struct People{
        uint256 fav_number;
        string name;
    }
    People public person = People({fav_number: 2, name: "Patrick"});

Arrays:
    Dyanamic Array = People[] public people;
        - In order to add people to the dynamic array we must push
            people.push(People("Todd", 55))
    Fixed Size = People[1] public people;

Maps:
    This is the same thing as a dictionary in python.
    mapping(string => uint256) public nameToFavoriteNumber;
        nameToFavoriteNumber["Becca"] = 24; #This will add the keyword Becca to the dictionary and set it to value 24.

Creating the Smart Contract on the Blockchain:
    - Make sure that the MetaMask extension is added on to chrome
    - Then change the enviroment to the Injected Web3, and MetaMask will take care of the execution process
    - Then Deploy and you will get a notification to continue from MetaMask
    - If you make changes to the contract by adding to the array or assigning a value(Orange icon), a new contract will be created
    - However if you make calls to the gray items, it will not result in a new contract, since we are just viewing a state

Storage Factory in Solidity:
    - importing a contract is done by file path:
        Given that SimpleStorage is in the same folder as StorageFactory:
            import "./SimpleStorage.sol"
    - Deploying a contract from another contract:
        contract StorageFactory{
            SimpleStorage[] public simpleStorageArray;
            function createSimpleStorageContract() public{
                SimpleStorage simpleStorage = new SimpleStorage();
                simpleStorageArray.push(simpleStorage);
            }
        }
    - When calling functions from another contract we need the address of the contract that we are interested in and the ABI(from the import) of the contract as well.
        SimpleStorage simpleStorage = SimpleStorage(address(simpleStorageArray[_simpleStorageIndex]));
        simpleStorage.store(_simpleStorageNumber);
    
Inheritence:
    - When you want the features of another class but also want to create objects of that same class in the new class, inheritence is a good way to go.
        contract StorageFactory is SimpleStorage{
            //Storage Factory will have all of the functions and variables from the Simple Storage
        }

Creating Payables:
    function fund() public payable {
         addressToAmountFunded[msg.sender] += msg.value;
        # msg.sender is the sender(address) of the function call and the msg.value is the amount that they sent

     }
     - In order to allow for deployment of payables we need to create a constructor for the payable in the first place 
     constructor () public  payable {
    }
     - When trying to add the value to an address make sure to place a number in the value location
     - Require:
        - When you need to make sure that the user is sending a valid amount of money make sure to have a require statement that checks it.
        - require(getConversionRate(msg.value)>=minimumUSD, "You need to spend more ETH!);
        - If it does not meet the requirement then it sends the user their money back

The Oracle Problem:
    - Smart contracts are unable to connect with external systems, data feeds, APIS, existing payment systems or any other off-chain resouyrces on their own.
    - Can't use an API because calling on different blocks might result in different returned valeus from the API resulting an error(No consensus)
    - Using a centralized oracle negates he benefits of smart contracts and introduces new privacy issues

Chainlink:
    - This solves the Oracle Problem by using a decentralized Oracle network

ABI:
    - To interact with a smart a deployed smart contract you will need an ABI
    - Interfaces compile down to an ABI
    - Always beed ab ABI to interact with a contract

Aggregator:
    - When dealing with different currencies in Solidity use the aggregator
    - import '@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol'
    https://docs.chain.link/docs/ethereum-addresses/

Library:
    - Similar to contracts however they are only deployed once at a specific addresss and their code is reused.

Withdraw:
    - In order to get the money back from that is stuck in the contract, we need to use a withdraw function
    - msg.sender.transfer(address(this).balance);
    - In order to make sure the money is going to the proper sender it is good idea to make variables
        require(msg.sender==owner);

Modifiers:
    - Aims to change the behaviour of a function wherever it is attached.
        modifier SomethingBefore {
            require(/* check something first */);
            _; // resume with function execution
        }
        // Do one where modifier is placed in the middle
        modifier SomethingAfter {
            _; // run function first
            require(/* then check something */)
}