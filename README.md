# About EOSIO

The EOS.IO software introduces a new blockchain architecture designed to enable vertical and horizontal scaling of decentralized applications. This is achieved by creating an operating system-like construct upon which applications can be built. The software provides accounts, authentication, databases, asynchronous communication and the scheduling of applications across many CPU cores or clusters. The resulting technology is a blockchain architecture that may ultimately scale to millions of transactions per second, eliminates user fees, and allows for quick and easy deployment and maintenance of decentralized applications, in the context of a governed blockchain.

# About this guide:

**Full documentation can be found at [https://developers.eos.io/](https://developers.eos.io/)**

This means your portal is correctly setup for the hackathon.

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

For this hackathon we will be using EOSIO v.1.3.2 (based on Docker image eosio/eos-dev:v1.3.2)

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

class example : public eosio::contract {
 public:
     using contract::contract;

     /// @abi action
     void hi( account_name user ) {
             print( "Hello, ", name{user} );
     }
};

EOSIO_ABI( example, (hi) )

```

Let's break this contract apart in parts.

```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>
```

This imports standard eosio c++ libraries. More libraries can be found in `eosiolib` folder.

```cpp
class example : public eosio::contract {
 public:
     using contract::contract;

     /// @abi action
     void hi( account_name user ) {
             print( "Hello, ", name{user} );
     }
};
```

This is a standard implementation of a contract structure that has one method called `hi` that takes a `user` parameter of type `account_name`. Then it prints out a name of this user.

```cpp
EOSIO_ABI( example, (hi) )
```

This line exposes the method `hi` to the ABI file. ABI file is like an address book that shows what are the methods and what are their parameters inside smart contract that can be called by your Dapp.

## Compile the smart contract

Let's create another handy alias for the smart contract compilator `eosiocpp`. 

```
alias eosiocpp='docker exec eosio_notechain_container eosiocpp'
```

The `eosiocpp` tool simplifies the work required to bootstrap a new contract. `eosiocpp` will create the two smart contract files with the basic skeleton to get you started. The skeleton file is the same `.cpp` file for the hello contract covered in the example above.

```
eosiocpp -n ${contractname}
```

Next, we need to generate a WASM file. A WASM file is a compiled smart contract ready to be uploded to EOSIO network.

```
eosiocpp -o /opt/eosio/bin/contracts/example/example.wast /opt/eosio/bin/contracts/example/example.cpp
```

Please note the path to the files is within Docker container, not your host machine, so please add `/opt/eosio/bin/contracts/` to point to the right contracts folder.

We now need to generate an ABI file:

```bash
eosiocpp -g /opt/eosio/bin/contracts/example/example.abi /opt/eosio/bin/contracts/example/example.cpp
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

class addressbook : public eosio::contract {
   public:
       
   private: 
  
};
```

Now let's define a new `struct` that will serve a structure of a row in our table in `private` as a private field:

```cpp
#include <eosiolib/eosio.hpp>

class addressbook : public eosio::contract {
   public:
       
   private: 
      struct record {
         account_name owner; // primary key                                      
         uint32_t     phone;
         std::string  fullname;
         std::string  address;

         uint64_t primary_key() const { return owner; }
         uint64_t by_phone() const    { return phone; }
      };
};
```

Now let's define the table itself and its indices: 

```cpp
   // We setup the table usin multi_index container:
   typedef eosio::multi_index<N(records), record,
      eosio::indexed_by<N(byphone), eosio::const_mem_fun<record, uint64_t, &record::by_phone> >
   > record_table;
   
   // Creating the instance of the `record_table` type                                                                             
   record_table _records;
```

We need to initialize the class in the constructor.

```cpp
// We inititialize the class with a constructor and we pass the account_name as a parameter in the constructor
// this account_name will be set to the account that deploys the contract
addressbook( account_name s ):
   contract(s),   // initialization of the base class for the contract
   _records(s, s) // initialize the table with code and scope NB! Look up definition of code and scope
{
}
```

Let's sum it all up in one file so far: 

```cpp
class addressbook : public eosio::contract {
   public:
      addressbook( account_name s ):
         contract( s ),   // initialization of the base class for the contract
         _records( s, s ) // initialize the table with code and scope NB! Look up definition of code and scope   
      {
      }
      
   private:
      // Setup the struct that represents a row in the table                                                            
      /// @abi table records                   
      struct record {
         account_name owner; // primary key                                      
         uint32_t     phone;
         std::string  fullname;
         std::string  address;

         uint64_t primary_key() const { return owner; }
         uint64_t by_phone() const    { return phone; }
      };

      typedef eosio::multi_index< N(records), record,
         eosio::indexed_by<N(byphone), eosio::const_mem_fun<record, uint64_t, &record::by_phone> >
      > record_table;

      // Creating the instance of the `record_table` type                      
      record_table _records;
};
	
```

Now let's create an action that adds a new record in our table: 


```cpp
/// @abi action
void create( account_name owner, uint32_t phone, const std::string& fullname, const std::string& address ) {

   require_auth( owner );

   // _records.end() is in a way similar to null and it means that the value isn't found
   // uniqueness of primary key is enforced at the library level but we can enforce it in the contract with a
   // better error message
   eosio_assert( _records.find( owner ) == _records.end(), "This record already exists in the addressbook" );

   eosio_assert( fullname.size() <= 20, "Full name is too long" );
   eosio_assert( address.size() <= 50, "Address is too long" );

   // we use phone as a secondary key                                                                                 
   // secondary key is not necessarily unique, we will enforce its uniqueness in this contract
   auto idx = _records.get_index<N(byphone)>();
   eosio_assert( idx.find( phone ) == idx.end(), "Phone number is already taken" );

   _records.emplace( owner, [&]( auto& rcrd ) {
      rcrd.owner    = owner;
      rcrd.phone    = phone;
      rcrd.fullname = fullname;
      rcrd.address  = address;
   });
}
```

And we need to expose our function to the ABI: 

```cpp
EOSIO_ABI( addressbook, (create) )
```

All together and add `remove` and `update` actions: 

```cpp
#include <eosiolib/eosio.hpp>

class addressbook : public eosio::contract {
   public:
      addressbook( account_name s ):
         contract( s ),   // initialization of the base class for the contract
         _records( s, s ) // initialize the table with code and scope NB! Look up definition of code and scope   
      {
      }

      /// @abi action
      void create( account_name owner, uint32_t phone, const std::string& fullname, const std::string& address ) {

         require_auth( owner );

         // _records.end() is in a way similar to null and it means that the value isn't found
         // uniqueness of primary key is enforced at the library level but we can enforce it in the contract with a
         // better error message
         eosio_assert( _records.find( owner ) == _records.end(), "This record already exists in the addressbook" );

         eosio_assert( fullname.size() <= 20, "Full name is too long" );
         eosio_assert( address.size() <= 50, "Address is too long" );

         // we use phone as a secondary key                                                                                 
         // secondary key is not necessarily unique, we will enforce its uniqueness in this contract
         auto idx = _records.get_index<N(byphone)>();
         eosio_assert( idx.find( phone ) == idx.end(), "Phone number is already taken" );

         _records.emplace( owner, [&]( auto& rcrd ) {
            rcrd.owner    = owner;
            rcrd.phone    = phone;
            rcrd.fullname = fullname;
            rcrd.address  = address;
         });
      }

      /// @abi action
      void remove( account_name owner ) {

         require_auth( owner );

         auto itr = _records.find( owner );
         eosio_assert( itr != _records.end(), "Record does not exit" );
         _records.erase( itr );
      }

      /// @abi action                                               
      void update( account_name owner, const std::string& address ) {

         require_auth( owner );

         auto itr = _records.find( owner );
         eosio_assert( itr != _records.end(), "Record does not exit" );
         eosio_assert( address.size() <= 50, "Address is too long" );
         _records.modify( itr, owner, [&]( auto& rcrd ) {
            rcrd.address = address;
         });
      }

   private:
      // Setup the struct that represents a row in the table                                                            
      /// @abi table records                   
      struct record {
         account_name owner; // primary key                                      
         uint32_t     phone;
         std::string  fullname;
         std::string  address;

         uint64_t primary_key() const { return owner; }
         uint64_t by_phone() const    { return phone; }
      };

      typedef eosio::multi_index< N(records), record,
         eosio::indexed_by<N(byphone), eosio::const_mem_fun<record, uint64_t, &record::by_phone> >
      > record_table;

      // Creating the instance of the `record_table` type                      
      record_table _records;
};

EOSIO_ABI( addressbook, (create)(remove)(update) )
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
eosiocpp -o /opt/eosio/bin/contracts/addressbook/addressbook.wasm /opt/eosio/bin/contracts/addressbook/addressbook.cpp
eosiocpp -g /opt/eosio/bin/contracts/addressbook/addressbook.abi /opt/eosio/bin/contracts/addressbook/addressbook.cpp
cleos set contract addressbook /opt/eosio/bin/contracts/addressbook -p addressbook@active
```

And let's add `khaled` to the database: 

```bash
cleos push action addressbook create '["khaled", 332233, "Khaled A", "Springfield"]' -p khaled
```

Looks good! Am I in?

```bash
cleos get table addressbook addressbook records
```

The result should look like this: 

```json
{
  "rows": [{
      "owner": "khaled",
      "phone": 332233,
      "fullname": "Khaled A",
      "address": "Springfield"
    }
  ],
  "more": false
}
```

What if we now need to update the address?

```bash
cleos push action addressbook update '["khaled", "Quahog"]' -p khaled
```

You should get the following result:

```json
{
  "rows": [{
      "owner": "khaled",
      "phone": 332233,
      "fullname": "Khaled A",
      "address": "Quahog"
    }
  ],
  "more": false
}
```

Congrats, all done! 
