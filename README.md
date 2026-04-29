# anode
import hashlib
import time
import json

# =============================================================================
# 1. Using SHA 256, obtain the message digest of string “Blockchain Developer”.
# =============================================================================
print("--- Question 1 ---")
message = "Blockchain Developer"
digest = hashlib.sha256(message.encode()).hexdigest()
print(f"Message: {message}")
print(f"SHA-256 Digest: {digest}\n")


# =============================================================================
# 2. Write a program to encrypt and decrypt the message “Hello World” using SHA256.
# NOTE: SHA-256 is a HASH function, not encryption. It is one-way. 
# You cannot "decrypt" it. Below we demonstrate hashing "Hello World".
# =============================================================================
print("--- Question 2 ---")
msg_2 = "Hello World"
hashed_msg = hashlib.sha256(msg_2.encode()).hexdigest()
print(f"Original: {msg_2}")
print(f"Hashed (SHA-256): {hashed_msg}")
print("Reminder: Hashes cannot be decrypted back to plain text.\n")


# =============================================================================
# 3. Implement RSA cryptographic algorithm
# =============================================================================
print("--- Question 3 ---")
# Simplified RSA Implementation
p, q = 61, 53
n = p * q
phi = (p-1) * (q-1)
e = 17  # Public exponent
d = 2753 # Private exponent (calculated via modular inverse)

def rsa_encrypt(m, e, n): return pow(m, e, n)
def rsa_decrypt(c, d, n): return pow(c, d, n)

msg_3 = 42
encrypted = rsa_encrypt(msg_3, e, n)
decrypted = rsa_decrypt(encrypted, d, n)
print(f"RSA Original: {msg_3} | Encrypted: {encrypted} | Decrypted: {decrypted}\n")


# =============================================================================
# 4, 6, 7, 8. Creating a Blockchain (PoW, 5 Nodes, History, Hash, Validity)
# =============================================================================
print("--- Questions 4, 6, 7, 8 ---")

class Block:
    def __init__(self, index, transactions, prev_hash):
        self.index = index
        self.timestamp = time.time()
        self.transactions = transactions
        self.prev_hash = prev_hash
        self.nonce = 0
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        block_string = json.dumps({
            "index": self.index,
            "timestamp": self.timestamp,
            "transactions": self.transactions,
            "prev_hash": self.prev_hash,
            "nonce": self.nonce
        }, sort_keys=True).encode()
        return hashlib.sha256(block_string).hexdigest()

    def mine_block(self, difficulty):
        target = "0" * difficulty
        while self.hash[:difficulty] != target:
            self.nonce += 1
            self.hash = self.calculate_hash()
        print(f"Block {self.index} Mined! Hash: {self.hash}")

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]
        self.difficulty = 2

    def create_genesis_block(self):
        return Block(0, "Genesis Block", "0")

    def add_block(self, transactions):
        new_block = Block(len(self.chain), transactions, self.chain[-1].hash)
        new_block.mine_block(self.difficulty)
        self.chain.append(new_block)

    def is_valid(self):
        for i in range(1, len(self.chain)):
            current = self.chain[i]
            previous = self.chain[i-1]
            if current.hash != current.calculate_hash(): return False
            if current.prev_hash != previous.hash: return False
        return True

# Initialize and add 5 blocks (nodes)
my_blockchain = Blockchain()
my_blockchain.add_block("Alice sent 1.0 BTC to Bob")
my_blockchain.add_block("Bob sent 0.5 BTC to Charlie")
my_blockchain.add_block("Charlie sent 0.2 BTC to Dave")
my_blockchain.add_block("Dave sent 0.1 BTC to Eve")

print(f"\nBlockchain Validity Check: {my_blockchain.is_valid()}\n")


# =============================================================================
# 5. Demonstrate sending of a digitally signed document
# =============================================================================
print("--- Question 5 ---")
document = "Important Contract Data"
# Sign using the RSA private key from Q3
signature = pow(int(hashlib.sha256(document.encode()).hexdigest(), 16), d, n)
print(f"Document: {document}")
print(f"Digital Signature: {signature}\n")


# =============================================================================
# 9. Implement a smart contract using solidity programming language
# =============================================================================
print("--- Question 9 ---")
solidity_code = """
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleContract {
    string public message;
    
    function setMessage(string memory _msg) public {
        message = _msg;
    }
}
"""
print("Solidity Code (displayed as string in Python):")
print(solidity_code)


# =============================================================================
# 10. Create a simple permissioned blockchain using Hyperledger Fabric.
# =============================================================================
print("--- Question 10 ---")
fabric_info = """
Hyperledger Fabric is not a single file script. It requires:
1. Docker Containers (Peers, Orderers, CAs)
2. A configtx.yaml (Network configuration)
3. Chaincode (The business logic)

Basic Organization structure in Fabric:
- Org1MSP (Member Service Provider)
- Peer0.org1.example.com
"""
print(fabric_info)
