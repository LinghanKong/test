ERC20
===

[![Version](http://img.shields.io/badge/Certik-randombytes.svg)](https://certik.org/) 
[![Version](http://img.shields.io/badge/Solidity-randombytes.svg)](https://solidity.readthedocs.io/en/v0.5.8/) 
[![Version](http://img.shields.io/badge/Ethereum-randombytes.svg)](https://www.ethereum.org/) 
[![Version](http://img.shields.io/badge/BlockChain-randombytes.svg)](https://solidity.readthedocs.io/en/v0.5.8/) 
[![Version](http://img.shields.io/badge/Cryptocurrency/ICO-randombytes.svg)](https://www.investopedia.com/terms/i/initial-coin-offering-ico.asp) 


##1. pre-request

 Before learning the ERC20, we should understand what is BlockChain, Ethereum and smart contract. 

###	-	Blockchain

Blockchain is a chain of blocks, but not in the traditional methods. By analyzing the word "Blockchain", "block" means the digital information and "chain" means public database. Therefore, we actually talk about the digital information in public database. To be more specfic, there are three main parts in blockchain:

*	Blocks store information about transaction scuh as, date, time and money amount of the most recent purchase from stores(any stores; it could be online or offline.) 
* 	Blocks store information that who are participating in transactions. For example, it could report the name and the website where the purchaser buy.
*  	Blocks store information that distinguishes them from other blocks that is using hash function to allow us to tell it apart from every other block. 

For some general informaion, a single block existing in the blockchain can store up to 1 MB of data which depends on the size of transaction. For more information of Blockchain, referencing at [IBM](https://www.ibm.com/blockchain/what-is-blockchain)

###	-	Smart Contract and solidity

A [smart contract](https://en.wikipedia.org/wiki/Smart_contract) is a computer code running on top of a blockchain containing a set of rules under which the parties to that smart contract agree to interact with each other. if the pre-defined requirements are meet, the agreement in the smart contract will automatically enforce. 
>Tips: Smart contract can only be as smart as the people coding taking into account all available information the time of coding

>While smart contracts have the potential to become legal contracts if certain conditions are met, they should not be confused with legal contracts accepted by courts and or law enforcement. However, we will probably see a fusion of legal contracts and smart contracts emerge over the next few years as the technology becomes more mature and widespread and legal standards are adopted.

#####	Solidity Smart Contract example

A contract in the sense of Solidity is a collection of code(function) and data(its state) which stay in a specifc address on the Ethereum. Let's take look a simple contract in Solidity document:

```javascripts

pragma solidity >=0.5.0 <0.7.0;

contract Coin {
    // The keyword "public" makes those variables
    // easily readable from outside.
    address public minter;
    mapping (address => uint) public balances;

    // Events allow light clients to react to
    // changes efficiently.
    event Sent(address from, address to, uint amount);

    // This is the constructor whose code is
    // run only when the contract is created.
    constructor() public {
        minter = msg.sender;
    }

    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        require(amount < 1e60);
        balances[receiver] += amount;
    }

    function send(address receiver, uint amount) public {
        require(amount <= balances[msg.sender], "Insufficient balance.");
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```

In the above example, [Pragmas](https://solidity.readthedocs.io/en/v0.5.8/layout-of-source-files.html#pragma) are coomen instructions for compilers about how to treat the source code. the first line expresses the source code is written for solidity version 0.4.0 or anything newer that does not break functionality (up to, but not including, version 0.7.0. ) 

```javascripts
address public minter;
```

The above code declares a [state variable](https://solidity.readthedocs.io/en/v0.5.3/structure-of-a-contract.html) which are "variables whose values are permanently stored in contract storage". In this case, the state variable named minter which is publicly accessible and the variable type is address which is is a 160-bit value and do not allow any arithmetic operations.

```javascripts
mapping(address=>uint) public balances;
```

The above line of code creates a public state variable, but it is a more complex datatype. This type maps addresses to unsigned integers.

Mappings can be seen as hash tables which virtually initialized such that every possible key exists from the start and is mapped to a value whose byte-representation is all zeros

```javascripts
function balances(address _account) external view returns (uint) {
    return balances[_account];
}
```
you can use the above function to query the balance of a single account.

```javascripts
event Sent(address from, address to, uint amount);
```
The above line declares a so-called "event" which is emitted in the last line of the function send. User-interface and server applications of course can listen for those events being emitted on the blockchain without much cost. As soon as it is emitted, the listener will receive the arguments, [from], [to] and [amount] which makes the track of transactions more easily. For more information to read about the listen for this event; reference at [Solidity document](https://solidity.readthedocs.io/en/v0.5.8/introduction-to-smart-contracts.html).

```javascript
    constructor() public {
        minter = msg.sender;
    }
```
The above case is a constructor which is a special function, running during creation of the contract and cannot be called afterwards. It permanently stores the address of the person creating the contract: msg (togetehr with `tx` and `block`) is a global variable that contains some properties which allow access to the blockchain. msg.sender is always the address where the current (external) function call came from.

Finally, the functions that will actually end up with the contract and can be called by users and contracts alike are mint and send. If mint is called by anyone except the account that created the contract, nothing will happen. This is ensured by the special function require which causes all changes to be reverted if its argument evaluates to false. The second call to require ensures that there will not be too many coins, which could cause overflow errors later.

On the other hand, send can be used by anyone (who already has some of these coins) to send coins to anyone else. If you do not have enough coins to send, the require call will fail and also provide the user with an appropriate error message string


###	-	Ethereum 

Ethereum is an open source, public, blockchain-based distributed computing platform and operating system featuring smart contract (scripting) functionality. It supports a modified version of Nakamoto consensus via transaction-based state transitions.

For example, Ether is a token whose blockchain is generated by the Ethereum platform. Ether can be transferred between accounts and used to compensate participant mining nodes for computations performed.[3] Ethereum provides a decentralized virtual machine, the Ethereum Virtual Machine (EVM), which can execute scripts using an international network of public nodes.[4] The virtual machine's instruction set, in contrast to others like Bitcoin Script, is thought to be Turing-complete. "Gas", an internal transaction pricing mechanism, is used to mitigate spam and allocate resources on the network.

For more information learn about [Ethereum](https://www.ethereum.org/).

	
##2. What is ERC20 Token Standard
The ERC20 token standard describes the functions and events that an Ethereum token contract has to implement.

##3. ERC20 Token Standard Interface

Following is an interface contract declaring the required functions and events to meet the ERC20 standard:


```javascript
contract ERC20Interface {
    function totalSupply() public view returns (uint);
    function balanceOf(address tokenOwner) public view returns (uint balance);
    function allowance(address tokenOwner, address spender) public view returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}
```
##4. Create your own Token based on ERC20 starndard

*	Starting a contract


There are different Visibility in solidity and there are differentiate among them. People can use them by defining new variable or function such as, `address private owner`

*	Public functions are part of the contract interface and can be either called internally or via messages. For public state variables, an automatic getter function (see below) is generated.
*	external:	 External functions are part of the contract interface, which means they can be called from other contracts and via transactions. An external function f cannot be called internally (i.e. f() does not work, but `this.f()` works). External functions are sometimes more efficient when they receive large arrays of data.
*	internal:	 Those functions and state variables can only be accessed internally (i.e. from within the current contract or contracts deriving from it), without using `this`.
* 	private: Private functions and state variables are only visible for the contract they are defined in and not in derived contracts.

There is another thing we need to learn which is `Mapping`

Mappping types are declared as `mapping(_KeyType => _ValueType)`. Here `_KeyType` can be almost any type except for a mapping, a dynamically sized array, a contract, an enum and a struct. `_ValueType` can actually be any type, including mappings.

Mappings can be seen as [hash atbles](https://en.wikipedia.org/wiki/Hash_table) which are virtually initialized such that every possible key exists and is mapped to a value whose byte-representation is all zeros: a type’s default value. The similarity ends here, though: The key data is not actually stored in a mapping, only its keccak256 hash used to look up the value.

example of mapping 
`mapping (uint256 => CampaignData) campaigns`

In the above example, uint256 is the `_keytype` and the campaignData is the `_valueType`. `CampaignData` is a struct.

`var campaign = campaigns[nextCampaignId];`
In this line, `nextCampaignId` is mapped as the `KeyType`, and the new `campaign` struct is the `_valueType`.


In the example, the name of mapping type is example, and it map

Try yourself:
In this example, there should be a mapping for balanacs for each account which should be private, another mapping for owner of account approves the transfer of an amount to another account wihch also should be private and an unsigned integers for store.


```javascripts
contract ERC20{

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

	// define an unsigned integers 256 which is private called _totalsupply
    uint256 private _totalSupply;
}
```

*	function totalSupply() public view returns (uint);

The next thing need to solve is that define the function. In solidity, a function declration in solidity looks like:

```javascripts
function example (string hi){

}
```
The above example is a function named example which take two parameters: a string.

>Note: It's convention (but not required) to start function parameter variable names with an underscore (_) in order to differentiate them from global variables. We'll use that convention throughout our tutorial.

In solidity, functions are public by default. This means anyone can call your contract's function and execute its code. However, it is not always desirable because the function may be more easily to attack. Thus, it is good practice to mark the functions as private by default and then only make public the function you want to expose to the world.

```javascript
uint[] numbers;

function _addToArray(uint _number) private {
  numbers.push(_number);
}

```
This means only other functions within our contract will be able to call this function and add to the numbers array.

##### Function return values function modifiers
To return a value from a function, the declaration looks like this:

```javascripts
string greeting = "What's up dog";

function sayHello() public returns (string) {
  return greeting;
}

```
Function modifiers:
The above function doesn't actually change state in Solidity — e.g. it doesn't change any values or write anything.

So in this case we could declare it as a view function, meaning it's only viewing the data but not modifying it:
```javascripts
function sayHello() public view returns (string) {
```
Solidity also contains pure functions, which means you're not even accessing any data in the app. Consider the following:

```javascripts
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
```
This function doesn't even read from the state of the app — its return value depends only on its function parameters. So in this case we would declare the function as pure.


Let's write another function which can return the total numer of tokens in existence.


```javascripts
function totalSupply() public view returns (uint256) {
        return _totalSupply;
}
```
The above function allows an instances of the contract to calculate and return the total amount of the token that exists in circulation.
references for [view](https://ethereum.stackexchange.com/questions/28898/when-to-use-view-and-pure-in-place-of-constant/28900) type;

*  function balanceOf(address tokenOwner) public view returns (uint balance);

For this function, it should handle the balance of the specifced address, and it should input a address which is to query the balance of and returns an uint256 representing the amunt owned the pass address.

```javascripts
function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }
```

This function allows a smart contract to store and return the balance of the provided address. The function accepts an address as a parameter, so it should be known that the balance of any address is public.



*  function allowance(address tokenOwner, address spender) public view returns (uint remaining);

The next function is to check the amount of tokens that an owner allowed to a spender.

```javascripts
function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }
```

this function is to check the amount of tokens that an owner allowed to a spender. There are two variables "owner": address The address which owns the funds and "spender": address The address which will spend the funds. It returns A uint256 specifying the amount of tokens still available for the spender.




*  transfer(address to, uint tokens)
This function is to transfer token for a specified addres and it needs two parameters. 

###  Requirements:
* - `recipient` cannot be the zero address.
* - the caller must have a balance of at least `amount`.

```javascripts
function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
```
This function lets the owner of the contract send a given amount of the token to another address just like a conventional cryptocurrency transaction.

*  approve(address spender, uint tokens)

this function approve the passed address to spend the specified amount of tokens on behalf of msg.sender.

> Beware that changing an allowance with this method brings the risk that someone may use both the old
> and the new allowance by unfortunate transaction ordering. One possible solution to mitigate this
> race condition is to first reduce the spender's allowance to 0 and set the desired value afterwards:

```javascripts
function approve(address spender, uint256 value) public returns (bool){
        _approve(msg.sender, spender, value);
        return true;
    }
```

When calling this function, the owner of the contract authorizes, or approves, the given address to withdraw instances of the token from the owner’s address.

Here, and in later snippets, you may see a variable msg . This is an implicit field provided by external applications such as wallets so that they can better interact with the contract. The Ethereum Virtual Machine (EVM) lets us use this field to store and process data given by the external application.

In this example, msg.sender is the address of the contract owner.

*  function transferFrom(address from, address to, uint tokens)

This function transfer tokens from one address to anotehr and it will need a "from" address, "to" address and a "value" which is uint256.

```javascripts
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount));
        return true;
    }
```

This function allows a smart contract to automate the transfer process and send a given amount of the token on behalf of the owner.

Seeing this might raise a few eyebrows. One may question why we need both transfer() and transferFrom() functions.

Consider transferring money to pay a bill. It’s extremely common to send money manually by taking the time to write a check and mail it to pay the bill off. This is like using transfer() : you’re doing the money transfer process yourself, without the help of another party.

In another situation, you could set up automatic bill pay with your bank. This is like using transferFrom() : your bank’s machines send money to pay off the bill on your behalf, automatically. With this function, a contract can send a certain amount of the token to another address on your behalf, without your intervention.

   
