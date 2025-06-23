# BlockChain

## Project Overview

BlockChain is a simple, educational blockchain implementation in C++. It demonstrates the core concepts of blockchain technology, including custom transactions, proof-of-work mining, and cryptographic signing using RSA. The project is designed for learning, experimentation, and as a foundation for more advanced blockchain applications.

---

## Features

- **Custom Transactions:** Easily extendable transaction class for various use cases.
- **Proof-of-Work Mining:** Adjustable mining difficulty to simulate real blockchain mining.
- **RSA Cryptography:** Each participant has an RSA key pair for signing and verifying transactions.
- **Genesis Block:** Automatic creation of the genesis block at chain initialization.
- **Mining Rewards:** Configurable mining rewards for incentivizing miners.
- **Balance Calculation:** Query balances for any address on the chain.
- **Exception Handling:** Custom exceptions for invalid transactions, key mismatches, and resigning attempts.
- **Modular Design:** Templated classes for blocks and transactions, allowing easy extension.

---

## Architecture

- **Blockchain:** A linked list of blocks, each containing a set of signed transactions.
- **Block:** Contains a set of transactions, a timestamp, a nonce (for proof-of-work), and cryptographic hashes linking to the previous block.
- **Transaction:** Represents a transfer of value, signed by the sender's private key and verifiable by others.
- **RSA Key Pair:** Used for digital signatures and verification, with keys generated from a list of prime numbers.
- **PrimeDB:** A file containing a list of prime numbers used for RSA key generation.

---

## File Structure

```
BlockChain-main/
├── block_chain.hpp        # Blockchain class template
├── block.hpp              # Block class template
├── transaction.hpp        # Base transaction class
├── exceptions.hpp         # Custom exception classes
├── main.cpp               # Example usage and entry point
├── makefile               # Build, run, and clean commands
├── includes/
│   ├── rsa.hpp            # RSA key pair and cryptography utilities
│   ├── prime.hpp          # Random prime number engine for RSA
│   └── primeDB            # List of prime numbers for RSA key generation
```

---

## Getting Started

### Requirements
- C++11 or later
- `g++` compiler (or compatible)

### Build the Project
```sh
make build
```

### Run the Demo
```sh
make run
```

### Clean Build Artifacts
```sh
make clean
```

### Build, Run, and Clean in One Step
```sh
make all
```

---

## Usage Example

The following is a simplified version of what happens in `main.cpp`:

```cpp
#include "block_chain.hpp"
#include "transaction.hpp"
#include "includes/rsa.hpp"

// Define your custom transaction by inheriting from ra::base_transaction
class custom_transaction : public ra::base_transaction {
  // ... (see main.cpp for full implementation)
};

int main() {
    ra::rsa_key_pair k1("user1");
    ra::rsa_key_pair k2("user2");

    ra::block_chain<custom_transaction, std::hash<std::string>> chain(MED_DIFFICULTY, ra::verify, 50);

    custom_transaction t1(k1.get_public_key(), k2.get_public_key(), 1000);
    t1.sign_transaction<ra::rsa_key_pair>(k1);
    chain.add_transaction(t1);
    chain.mine_pending_transactions(k1.get_public_key());

    std::cout << "k1 balance: " << chain.get_balance(k1.get_public_key()) << std::endl;
    std::cout << "k2 balance: " << chain.get_balance(k2.get_public_key()) << std::endl;
    std::cout << "Is chain valid? " << chain.is_chain_valid() << std::endl;
}
```

---

## Customization

- **Transaction Types:**
  - Inherit from `ra::base_transaction` to define your own transaction logic.
  - Implement required methods: `generate_hash_input`, `operator<`, `get_balance`, `sign_transaction`, and `is_transaction_valid`.
- **Hash Function:**
  - The block and blockchain classes are templated; you can use a different hash function (e.g., SHA256) by providing your own functor.
- **Mining Difficulty & Reward:**
  - Set these parameters when constructing the blockchain object in `main.cpp`.
- **PrimeDB:**
  - The file `includes/primeDB` must be present and contain a list of prime numbers (one per line) for RSA key generation.

---

## Security Notes

- This project is for educational purposes only and **not** suitable for production or real-world financial use.
- The cryptography and proof-of-work are simplified and not secure against real attacks.
- Do not use this code to store or transfer real value.

---

## License

This project is released under the MIT License.

---

## Contact

For questions, suggestions, or contributions, please open an issue or contact the project maintainer. 
