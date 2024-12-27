# Enhancing-Our-Blockchain-with-Proof-of-Work__2

proof-of-work mechanism to our blockchain to ensure the integrity and security of the data.





Learning Outcome:
Understand the concept of proof-of-work and its role in ensuring blockchain security.




import hashlib
import datetime as dt

class Block:
    def __init__(self, index, timestamp, data, previous_hash=''):
        self.index = index
        self.timestamp = timestamp
        self.data = data
        self.previous_hash = previous_hash
        self.nonce = 0  # Added for Proof-of-Work
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        sha = hashlib.sha256()
        sha.update(str(self.index).encode('utf-8') +
                   str(self.timestamp).encode('utf-8') +
                   str(self.data).encode('utf-8') +
                   str(self.previous_hash).encode('utf-8') +
                   str(self.nonce).encode('utf-8'))
        return sha.hexdigest()

    def mine_block(self, difficulty):
        target = '0' * difficulty
        while self.hash[:difficulty] != target:
            self.nonce += 1
            self.hash = self.calculate_hash()
        print(f"Block mined: {self.hash}")

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]
        self.difficulty = 4

    def create_genesis_block(self):
        return Block(0, dt.datetime.now(), "Genesis Block", "0")

    def get_latest_block(self):
        return self.chain[-1]

    def add_block(self, new_block):
        new_block.previous_hash = self.get_latest_block().hash
        new_block.mine_block(self.difficulty)  # Mine the new block
        self.chain.append(new_block)

# Create blockchain and add blocks
blockchain = Blockchain()
print("Mining block 1...")
blockchain.add_block(Block(1, dt.datetime.now(), {"amount": 4}))
print("Mining block 2...")
blockchain.add_block(Block(2, dt.datetime.now(), {"amount": 10}))

# Print the blockchain
for block in blockchain.chain:
    print(f"Block {block.index}:\nHash: {block.hash}\nPrevious Hash: {block.previous_hash}\nData: {block.data}\nNonce: {block.nonce}\n")

