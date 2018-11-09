# About EOSIO

The EOS.IO software introduces a new blockchain architecture designed to enable vertical and horizontal scaling of decentralized applications. This is achieved by creating an operating system-like construct upon which applications can be built. The software provides accounts, authentication, databases, asynchronous communication and the scheduling of applications across many CPU cores or clusters. The resulting technology is a blockchain architecture that may ultimately scale to millions of transactions per second, eliminates user fees, and allows for quick and easy deployment and maintenance of decentralized applications, in the context of a governed blockchain.

# About this guide:

**Full documentation can be found at [https://developers.eos.io/](https://developers.eos.io/)**

The purpose of this document is to setup you development environment in an agile manner and prepare you for the development of your project ideas.

Let's start creating a local EOSIO network that is not connected to the public network and will be under your full control. It will simulate a close to real network environment.

This guide assumes working on macos or Linux.

# Required knowledge:

## Smart contracts

To develop smart contracts on EOSIO you will need following skills:

### C / C++ Experience

EOSIO based blockchains execute user-generated applications and code using WebAssembly (WASM). WASM is an emerging web standard with widespread support of Google, Microsoft, Apple, and others. At the moment the most mature toolchain for building applications that compile to WASM is clang/llvm with their C/C++ compiler. For best compatibility, it is recommended that you use the EOSIO toolchain.

Other toolchains in development by 3rd parties include: Rust, Python, and Solidity. While these other languages may appear simpler, their performance will likely impact the scale of application you can build. We expect that C++ will be the best language for developing high-performance and secure smart contracts and plan to use C++ for the foreseeable future.

### Linux / Mac OS Experience

The EOSIO software supports the following environments:

* Amazon 2017.09 and higher
* Centos 7
* Fedora 25 and higher (Fedora 27 recommended)
* Mint 18
* Ubuntu 16.04 (Ubuntu 16.10 recommended)
* Ubuntu 18.04
* MacOS Darwin 10.12 and higher (MacOS 10.13.x recommended)

### Version of EOSIO

For this hackathon we will be using EOSIO v.1.4.2 (based on Docker image eosio/eos-dev:v1.4.2) and EOSIO.CDT v.1.3.2

### Command Line Knowledge

There are a variety of tools provided along with EOSIO which requires you to have basic command line knowledge in order to interact with.

## Dapp integration

To integrate your dApp with EOSIO you will need some Javascript experience. To interact with EOSIO from the frontend, we’ll use EOSJS library.

# Install Docker:

To start, we need to have Docker installed on your computer. 

Docker is a container management service. The keywords of Docker are "develop", ship and run anywhere". The whole idea of Docker is for developers to easily develop applications, ship them into containers which can then be deployed anywhere.

To install Docker please follow this guide: [https://docs.docker.com/install/](https://docs.docker.com/install/)

## C++ coding environment setup

We can use any text editor that, preferrably, supports c++ highlighting. One of the popular editors is Sublime Text. Another option is an IDE, which provides a more sophisticated code completion and better development experience.

### Install Sublime:
[https://www.sublimetext.com/](https://www.sublimetext.com/)

# Start your dev environment:

## EOSIO example application:

For the purposes of the hackathon we are recommending you to use an example application our team has built and build on top of it. It includes a Docker container with a single node blockchain and a React frontend with some preinstalled libraries that will help you to start developing in no time.

NoteChain demonstrates the eosio platform running a blockchain as a local single node test net with a simple DApp, NoteChain. NoteChain allows users to create and update notes. 

Clone the example app repository:

```bash
cd ~
git clone https://github.com/EOSIO/eosio-project-boilerplate-simple
cd eosio-project-boilerplate-simple
```

and run

```bash
./quick_start.sh
```

After a short setup process your browser should automatically open a new tab on `http://localhost:3000/`

Copy one of the example accounts information to the UI of the notechain application:

```
{
    "name": "useraaaaaaaa", // This should go to an Account field
    "privateKey": "5K7mtrinTFrVTduSxizUc5hjXJEtTjVTsqSHeBHes1Viep86FP5", // This should go to a Private Key fields
    "publicKey": "EOS6kYgMTCh1iqpq9XGNQbEi8Q6k5GujefN9DSs55dcjVyFAq7B6b"
  }
```

add some test copy in the Note field and press 'Add/Update Note'. As a result, you should get a new note created in the frontend of the application.

Congrats! You have a very simple single node blockchain and a simple application running on your computer!

Also go to this address in the browser to check that RPC interface is working: `http://localhost:8888/v1/chain/get_info`

You should see a message similar to following:

```json
{
    "server_version": "75635168",
    "chain_id": "cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f",
    "head_block_num": 2511,
    "last_irreversible_block_num": 2510,
    "last_irreversible_block_id": "000009ce07f8934fd8a6498e120b36b7eb012896481f5102f8cf3d9ec1c03775",
    "head_block_id": "000009cfc8a2adf575d78218b28695615bdb6724a6e40359f96c9e1395386b14",
    "head_block_time": "2018-08-01T06:42:39.500",
    "head_block_producer": "eosio",
    "virtual_block_cpu_limit": 2458387,
    "virtual_block_net_limit": 12913257,
    "block_cpu_limit": 199900,
    "block_net_limit": 1048576
}

```

More in-depth documentation for the example app with additional commands can be found here: https://github.com/EOSIO/eosio-project-boilerplate-simple

# Cleos

`cleos` is a command line interface to interact with the blockchain and to manage wallets.

For convenience we will create a bash alias for cleos running inside our container. In terminal, run:

```bash
alias cleos='docker exec eosio_notechain_container /opt/eosio/bin/cleos -u http://localhost:8888'
```

Now try to run `cleos --help` in your Terminal. You should see a following output:

```
Command Line Interface to EOSIO Client
Usage: /opt/eosio/bin/cleos [OPTIONS] SUBCOMMAND

Options:
  -h,--help                   Print this help message and exit
  -u,--url TEXT=http://127.0.0.1:8888/
                              the http/https URL where nodeos is running
  --wallet-url TEXT=http://127.0.0.1:8900/
                              the http/https URL where keosd is running
  -r,--header                 pass specific HTTP header; repeat this option to pass multiple headers
  -n,--no-verify              don't verify peer certificate when using HTTPS
  -v,--verbose                output verbose actions on error
  --print-request             print HTTP request to STDERR
  --print-response            print HTTP response to STDERR

Subcommands:
  version                     Retrieve version information
  create                      Create various items, on and off the blockchain
  get                         Retrieve various items and information from the blockchain
  set                         Set or update blockchain state
  transfer                    Transfer EOS from account to account
  net                         Interact with local p2p network connections
  wallet                      Interact with local wallet
  sign                        Sign a transaction
  push                        Push arbitrary transactions to the blockchain
  multisig                    Multisig contract commands
  sudo                        Sudo contract commands
  system                      Send eosio.system contract action to the blockchain.
```

More on `cleos` you can read here: [https://developers.eos.io/eosio-nodeos/docs/cleos-overview](https://developers.eos.io/eosio-nodeos/docs/cleos-overview)

Before getting to the next section, please also read https://developers.eos.io/eosio-nodeos/docs/accounts-and-permissions to familiarize yourself with the concepts of accounts, wallets and permissions in EOSIO.

## Wallets

The wallet can be thought of as a repository of public-private key pairs. These are needed to sign operations performed on the blockchain. Wallets and their content are managed by keosd. Wallets are accessed using cleos.

Let's create our first wallet:

```bash
cleos wallet create --to-console
```

The output of this command will give you a password. Save this password - we will need it later.

To work with a wallet we need to open and unlock it:

```bash
cleos wallet open
cleos wallet unlock --password YOURPASSWORD
```

Where `YOURPASSWORD` is a password generated from the `create` command.

Create active and owner keys:

```bash
cleos create key --to-console
cleos create key --to-console
```

By labeling our new keys as follows, you’ll be a lot less likely to get the keys mixed up as you develop (just save it somewhere safe):

```bash
eosio Private key: 5KXuzu7QPjjEpTf22TvZ9ojQKjo1JVhfL5Lv6kSBX3v79GL5SL2
eosio Public key: EOS5UJuKQbmfzizeDXNikPbeCwYBpAq1vUUBuf7seJEpCoLzaUt4k
eosio Private key: 5JMa3mqbxn3WQDRCQZPVD3ny25pxydmwUYrC1MMDLU3Hn1d4vTk
eosio Public key: EOS6eY64pnpSZn2jQSPzs9X56j2JJbbe7sSDxL1PwwvwYytb9ETFu
```

Now lets import private keys into our wallet:

```bash
cleos wallet import --private-key 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
cleos wallet import --private-key 5JKrSzsuztAPvTzghi9VU4522sT49SeE3XVHbB8HsfC3ikifJRf
```

Now lets check the wallet is there:

```bash
cleos wallet list
```

The output should be following:

```
Wallets:
[
  "default *",
  "eosiomain",
  "notechainwal"
]
```

The * shows which wallets are open.

And that keys are imported in the wallet:

```bash
cleos wallet list keys
```

With the successful output: 

```
Wallets:
[
  "default *",
  "eosiomain",
  "notechainwal"
]
[
  "EOS7tLC1C4GPoU9rNW16zZFPpRf5887J2mfRTB8hWGQz1LVsyrBRW"
]
```

Awesome! You are up and running.

## User account

Now lets create some user accounts.

Each account should have a Private key. Think of it as both login and password for your EOSIO account.

Generate key pairs first:

```bash
cleos create key --to-console
cleos create key --to-console
```

Save the keys in a separate file. We will need them later.

```
testacc owner Public Key: "EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
testacc owner Private Key: "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"

testacc active Public Key: "EOS7EzCEh94uN2k59wznzsZDcFVnpZ3wuiYvPSbb8bXDS6U7twKQF",
testacc active Public Key: "5JKrSzsuztAPvTzghi9VU4522sT49SeE3XVHbB8HsfC3ikifJRf"
```

Now let's import the keys in the wallet:

```bash
cleos wallet import --private-key PRIVATEKEYOWNER
cleos wallet import --private-key PRIVATEKEYACTIVE
```

To create a new account we need to open the eosiomain wallet and then run a cleos command:

```bash
cleos create account eosio testacc pubkey1 pubkey2
cleos get account testacc -j
```

# Smart contract

Now it's time to create your first smart contract!

The folder where you should store your smart contracts is following:

```bash
cd ~/eosio-project-boilerplate-simple/eosio_docker/contracts
```

We need to create a new folder for our example contract.

```bash
mkdir example
cd example
touch example.cpp
subl ./example.cpp
```

Our example contract will be very simple. It will take a username as an argument and will publish a "Hello, username" message to the blockchain. It's quite useless, but good for a demo. We hope your smart contracts will be more sophisticated :)

Open `example.cpp` file in your editor and paste following code:

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

using namespace eosio;

class [[eosio::contract]] example : public contract {
  public:
      using contract::contract;

      [[eosio::action]]
      void hi( name user ) {
         print( "Hello, ", name{user});
      }
};
EOSIO_DISPATCH( example, (hi))

```

Let's break this contract apart in parts.

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>
```

This imports standard eosio c++ libraries. More libraries can be found in `eosiolib` folder.

EOSIO contracts extend the contract class. Initialize our parent contract class with the code name of the contract and the receiver. The important parameter here is the code parameter which is the account on the blockchain that the contract is being deployed to.

```cpp
class hello : public contract {
  public:
      using contract::contract;

      [[eosio::action]]
      void hi( name user ) {
         print( "Hello, ", name{user});
      }
};
```

This is a standard implementation of a contract structure that has one method called `hi` that takes a `user` parameter of type `name`. Then it prints out a name of this user.

```cpp
EOSIO_DISPATCH( example, (hi))
```

This line exposes the method `hi` to the ABI file. ABI file is like an address book that shows what are the methods and what are their parameters inside smart contract that can be called by your app.

## EOSIO.CDT

EOSIO.CDT is a toolchain for WebAssembly (WASM) and set of tools to facilitate contract writing and compilation for the EOSIO platform.

EOSIO.CDT currently supports Mac OS X brew, Linux x86_64 Debian packages, and Linux x86_64 RPM packages.

**If you have previously installed EOSIO.CDT, please run the `uninstall` script (it is in the directory where you cloned EOSIO.CDT) before downloading and using the binary releases.**

### Mac OS X Brew Install
```sh
$ brew tap eosio/eosio.cdt
$ brew install https://raw.githubusercontent.com/EOSIO/homebrew-eosio.cdt/50f00447765854f6e4e3b2d4ef36324cc38e5362/eosio.cdt.rb 
```

For more installation options for other systems please follow a guide here: [https://github.com/EOSIO/eosio.cdt/blob/master/README.md](https://github.com/EOSIO/eosio.cdt/blob/master/README.md)

## Compile the smart contract

First we need to generate a WASM file. A WASM file is a compiled smart contract ready to be uploded to EOSIO network.

`eosio-cpp` is the WASM compiler and an ABI generator utility. Before uploading the smart contract to the network we need to compile it from C++ to WASM.

```
eosio-cpp -o ~/eosio-project-boilerplate-simple/eosio_docker/contracts/example/example.wasm ~/eosio-project-boilerplate-simple/eosio_docker/contracts/example/example.cpp --abigen --contract example
```

Now in the folder `` you will see three files:

```bash
example.cpp  # this is source code of the example contract
example.abi  # this is the ABI file - public interface in the smart contract
example.wasm # this is the compiled WASM file
```

Congratulations! You have created your first smart contract! Lets upload this contract to the blockchain:

```bash
cleos set contract testacc /opt/eosio/bin/contracts/example/ --permission testacc@active
```

Run the transaction:

```bash
cleos push action testacc hi '["testacc"]' -p testacc@active
```

## EOSIO token contract

Now let's get real and create a custom token. With EOSIO it's easy!

First, we need to create an account for currency system contract:

```bash
cleos create key --to-console
cleos create key --to-console
cleos wallet import --private-key **PRIVATEKEY1**
cleos wallet import --private-key **PRIVATEKEY2**
cleos create account eosio eosio.token **PUBLICKEY1** **PUBLICKEY2**
```

Then we need to upload the smart contract:

```bash
cleos set contract eosio.token /contracts/eosio.token -p eosio.token
```

Once that done, we can issue new token!

```bash
cleos push action eosio.token create '{"issuer":"eosio", "maximum_supply":"1000000000.0000 HAK"}' -p eosio.token@active
```

This command created a new token `HAK` with a precision of 4 decimals and a maximum supply of `1000000000.0000 HAK`.

In order to create this token we required the permission of the eosio.token contract because it "owns" the symbol namespace (e.g. "HAK"). Future versions of this contract may allow other parties to buy symbol names automatically. For this reason we must pass `-p eosio.token` to authorize this call.

## Issue Tokens to Account "testacc"

Now that we have created the token, the issuer can issue new tokens to the account user we created earlier.

We will use the positional calling convention (vs named args).

```
cleos push action eosio.token issue '[ "testacc", "100.0000 HAK", "memo" ]' -p eosio
```

This time the output contains several different actions: one issue and three transfers. While the only action we signed was issue, the issue action performed an "inline transfer" and the "inline transfer" notified the sender and receiver accounts. The output indicates all of the action handlers that were called, the order they were called in, and whether or not any output was generated by the action.

Technically, the eosio.token contract could have skipped the inline transfer and opted to just modify the balances directly. However, in this case, the eosio.token contract is following our token convention that requires that all account balances be derivable by the sum of the transfer actions that reference them. It also requires that the sender and receiver of funds be notified so they can automate handling deposits and withdrawals.

Let's check `testacc`'s balance:

```bash
cleos get table eosio.token testacc accounts
```

You should see following output:

```json
{
  "rows": [{
      "balance": "100.0000 HAK"
    }
  ],
  "more": false
}
```

Now, let's send some tokens to another user! 

```bash
cleos push action eosio.token transfer '[ "testacc", "eosio", "25.0000 HAK", "m" ]' -p testacc@active
```

Nailed it! Let's check the balance is correct:

```bash
cleos get table eosio.token eosio accounts
```

Should give you: 

```json
{
  "rows": [{
      "balance": "25.0000 HAK"
    }
  ],
  "more": false
}
```

```bash
cleos get table eosio.token testacc accounts
```

Should give you: 

```json
{
  "rows": [{
      "balance": "75.0000 HAK"
    }
  ],
  "more": false
}
```

Awesome! Let's move to the next part.

## Persistence API

Great, now we want to store our information in a table-like structure, similar to a database. 

Let's imagine we are building an address book where users can add their social security number, age and name. 

First, create a folder in your `work` folder that will contain the contract files.

```bash
cd ~/eosio-project-boilerplate-simple/eosio_docker/contracts
mkdir addressbook
cd addressbook
```

And create a new `.cpp` file:

```bash
touch addressbook.cpp
subl ./addressbook.cpp
```

Let's create a standard structure for a contract file:

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

using namespace eosio;

class addressbook : public eosio::contract {
  public:
       
  private: 
  
};
```

Before a table can be configured and instantiated, a struct that represents the data structure of the address book needs to be written. Think of this as a "schema." Since it's an address book, the table will contain people, so create a struct called "person"

```cpp
struct person {};
```

When defining the schema for a `multi_index` table, you will require a unique value to use as the primary key.

For this contract, use a field called "key" with type name. This contract will have one unique entry per user, so this key will be a consistent and guaranteed unique value based on the user's name.

```cpp
struct person {
	name key;
};
```

Since this contract is an address book it probably should store some relevant details for each entry or person.

```cpp
struct person {
  name key;
  std::string first_name;
  std::string last_name;
  std::string street;
  std::string city;
  std::string state;
  uint32_t phone;
};
```

Great. The basic schema is now complete. Next, define a `primary_key` method, which will be used by `multi_index` iterators. Every multi_index schema requires a primary key. To accomplish this you simply create a method called `primary_key()` and return a value, in this case, the `key` member as defined in the struct.


```cpp
struct person {
  name key;
  std::string first_name;
  std::string last_name;
  std::string street;
  std::string city;
  std::string state;
  uint64_t phone;

  uint64_t primary_key() const { return key.value;}
};
```

*Note: A table's schema cannot be modified while it has data in it. If you need to make changes to a table's schema in any way, you first need to remove all its rows*


Add the `struct` in `private` as a private field:

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

class addressbook : public eosio::contract {
   public:
   	using contract::contract;
       
   private:
   	struct person {
		 name key;
		 std::string first_name;
		 std::string last_name;
		 std::string street;
		 std::string city;
		 std::string state;
		 uint64_t phone;
		 
		 uint64_t primary_key() const { return key.value;}
	};
};
```

Now that the schema of the table has been defined with a struct we need to configure the table. The `eosio::multi_index` constructor needs to be named and configured to use the struct we previously defined.

```cpp
// We setup the table usin multi_index container:
typedef eosio::multi_index<"people"_n, person> addressbook_type;
```

We need to initialize the class in the constructor.

```cpp
// We inititialize the class with a constructor and we pass the name as a parameter in the constructor
// this name will be set to the account that deploys the contract
addressbook(name receiver, name code,  datastream<const char*> ds):contract(receiver, code, ds) {}
```

Let's sum it all up in one file so far: 

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

class addressbook : public eosio::contract
{
  public:
    using contract::contract;
    addressbook(name receiver, name code, datastream<const char *> ds) : contract(receiver, code, ds) {}

  private:
    struct person
    {
        name key;
        std::string first_name;
        std::string last_name;
        std::string street;
        std::string city;
        std::string state;

        uint64_t primary_key() const { return key.value; }
    };

    typedef eosio::multi_index<"people"_n, person> addressbook_type;

};	
```

Next, define an action for the user to add or update a record. This action will need to accept any values that this action needs to be able to emplace (create) or modify.


```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state, uint64_t phone) {
}
```

Earlier, it was mentioned that only the user has control over their own record, as this contract is opt-in. To do this, utilize the require_auth method provided by the eosio.cdt. This method accepts one argument, an name type, and asserts that the account executing the transaction equals the provided value.

```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state, uint64_t phone) {
 require_auth( user );
}
```

Instantiate the table. Previously, a `multi_index table` was configured, and declared it as `addressbook_type `. To instantiate a table, consider its two required arguments:

* The `"code"`, which represents the contract's account. This value is accessible through the scoped `_code` variable.
* The `"scope"` which make sure the uniqueness of the contract. In this case, since we only have one table we can use `"_code"` as well. Important to notice, we are passing `_code.value` that returns `_code` in `unit64_t` as that is what `scope` requires.

```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state, uint64_t phone) {
  require_auth( user );
  addressbook_type addresses(_code, _code.value);
}
```

Next, query the iterator, setting it to a variable since this iterator will be used several times.

```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state, uint64_t phone) {
  require_auth( user );
  addressbook_type addresses(_code, _code.value);
  auto iterator = addresses.find(user.value);
}
```

Now our function needs to actually add or update the record (if it already exists) in the table:

```cpp
if( iterator == addresses.end() )
  {
    //The user isn't in the table
  }
  else {
    //The user is in the table
  }
```

If the record doesn't exist, we need to create it. We will use an `emplace` method to do it:

```cpp
addresses.emplace(user, [&]( auto& row ) {
    row.key = user;
    row.first_name = first_name;
    row.last_name = last_name;
    row.street = street;
    row.city = city;
    row.state = state;
    row.phone = phone;
});
```

If it already exists - we will update it using `modify` method:

```cpp
addresses.modify(iterator, user, [&]( auto& row ) {
    row.key = user;
    row.first_name = first_name;
    row.last_name = last_name;
    row.street = street;
    row.city = city;
    row.state = state;
    row.phone = phone
});
```

Let's put it all together:

```cpp

#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

class addressbook : public eosio::contract
{
  public:
    using contract::contract;
    addressbook(name receiver, name code, datastream<const char *> ds) : contract(receiver, code, ds) {}

    void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state, uint64_t phone)
    {
        require_auth(user);
        addressbook_type addresses(_code, _code.value);
        auto iterator = addresses.find(user.value);

        if (iterator == addresses.end())
        {
            //The user isn't in the table
            addresses.emplace(user, [&](auto &row) {
                row.key = user;
                row.first_name = first_name;
                row.last_name = last_name;
                row.street = street;
                row.city = city;
                row.state = state;
                row.phone = phone;
            });
        }
        else
        {
            //The user is in the table
            addresses.modify(iterator, user, [&](auto &row) {
                row.key = user;
                row.first_name = first_name;
                row.last_name = last_name;
                row.street = street;
                row.city = city;
                row.state = state;
                row.phone = phone;
            });
        }
    }

  private:
    struct person
    {
        name key;
        std::string first_name;
        std::string last_name;
        std::string street;
        std::string city;
        std::string state;
        uint64_t phone;

        uint64_t primary_key() const { return key.value; }
    };

    typedef eosio::multi_index<"people"_n, person> addressbook_type;

```

We also may want to add the `erase` method. Please remember it doesn't remove the record from the history, but removes it from the current state of the database, freeing resources if you are on a resource-limited network.

```cpp
void erase(name user) {
    require_auth(user);
    addressbook_type addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    eosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
}
```

The contract is now mostly complete. Users can create, modify and erase records. However, the contract is not quite ready to be compiled.

At the bottom of the file, utilize the `EOSIO_DISPATCH` macro, passing the name of the contract, and our lone action "upsert".

This macro handles the apply handlers used by wasm to dispatch calls to specific methods in our contract.

Adding the following to the bottom of `addressbook.cpp` will make our `cpp` file compatible with EOSIO's wasm interpreter. Failing to include this declaration may result in an error when deploying the contract.

```cpp
EOSIO_DISPATCH( addressbook, (upsert)(erase))
```

## ABI Type Declarations

`eosio.cdt` includes an ABI Generator, but for it to work will require some minor declarations to our contract.

There are three main types of the ABI annotation that you need to use in order for the ABI Generator to recognise relevant functions and export them to the ABI file correctly:

```cpp
[[eosio::contract]] # This annotation is needed at the contract class definition
[[eosio::action]] # This annotation is needed on the publicly available functions
[[eosio::table]] # This annotation is needed for the multi index table structs
```

The final version of our file will look like this: 

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

using namespace eosio;

class [[eosio::contract]] addressbook : public eosio::contract {

public:
  using contract::contract;
  
  addressbook(name receiver, name code,  datastream<const char*> ds): contract(receiver, code, ds) {}

  [[eosio::action]]
  void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state, uint64_t phone) {
    require_auth( user );
    addressbook_type addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    if( iterator == addresses.end() )
    {
      addresses.emplace(user, [&]( auto& row ) {
       row.key = user;
       row.first_name = first_name;
       row.last_name = last_name;
       row.street = street;
       row.city = city;
       row.state = state;
       row.phone = phone;
      });
    }
    else {
      std::string changes;
      addresses.modify(iterator, user, [&]( auto& row ) {
        row.key = user;
        row.first_name = first_name;
        row.last_name = last_name;
        row.street = street;
        row.city = city;
        row.state = state;
        row.phone = phone;
      });
    }
  }

  [[eosio::action]]
  void erase(name user) {
    require_auth(user);

    addressbook_type addresses(_self, _code.value);

    auto iterator = addresses.find(user.value);
    eosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
  }

private:
  struct [[eosio::table]] person {
    name key;
    std::string first_name;
    std::string last_name;
    std::string street;
    std::string city;
    std::string state;
    uint64_t phone;
    uint64_t primary_key() const { return key.value; }

  };
  typedef eosio::multi_index<"people"_n, person> addressbook_type;

};

EOSIO_DISPATCH( addressbook, (upsert)(erase))
```

Awesome, we have our table! Let's test it now.

First, we need to create couple of accounts:

```bash
# This account will be a user who wants to add their contact details to the address book
cleos create key --to-console
cleos create key --to-console
cleos wallet import --private-key **PRIVATEKEY1**
cleos wallet import --private-key **PRIVATEKEY2**
cleos create account eosio khaled **PUBLICKEY1** **PUBLICKEY2**

# This account will be used to store the smart contract for the address book
cleos create key --to-console
cleos create key --to-console
cleos wallet import --private-key **PRIVATEKEY3**
cleos wallet import --private-key **PRIVATEKEY4**
cleos create account eosio addressbook **PUBLICKEY3** **PUBLICKEY4**
```

Now we need to compile and upload the smart contract:

```bash
eosio-cpp -o ~/eosio-project-boilerplate-simple/eosio_docker/contracts/addressbook/addressbook.wasm ~/eosio-project-boilerplate-simple/eosio_docker/contracts/addressbook/addressbook.cpp --abigen --contract addressbook

# notice we are using the path to the contract inside the container, not the local path
cleos set contract addressbook /opt/eosio/bin/contracts/addressbook -p addressbook@active
```

And let's add `khaled` to the database: 

```bash
cleos push action addressbook upsert '["khaled", "Khaled A", "Hass", "Springfield St", "San Francisco", "CA", 123456]' -p khaled
```

Looks good! Is Khaled in?

```bash
cleos get table addressbook addressbook people
```

The result should look like this: 

```json
{
  "rows": [{
      "key": "khaled",
      "first_name": "Khaled A",
      "last_name": "Hass",
      "street": "Springfield St",
      "city": "San Francisco",
      "state": "CA",
      "phone": 123456
    }
  ],
  "more": false
}
```

What if we now need to update the first name?

```bash
cleos push action addressbook upsert '["khaled", "Josh", "Hass", "Springfield St", "San Francisco", "CA", 123456]' -p khaled
```

You should get the following result by repeating the last cleos command:

```json
{
  "rows": [{
      "key": "khaled",
      "first_name": "Josh",
      "last_name": "Hass",
      "street": "Springfield St",
      "city": "San Francisco",
      "state": "CA",
      "phone": 123456
    }
  ],
  "more": false
}
```

Congrats, all done! 
