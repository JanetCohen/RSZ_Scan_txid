# Bitcoin Private Key Finder from Blockchain Transactions

This Python script scans the Bitcoin blockchain for specific transaction patterns and attempts to recover private keys using a mathematical vulnerability known as the reuse of `r` values in the ECDSA signatures. Once a vulnerable private key is discovered, it checks the balance of the corresponding Bitcoin address and saves the results to a file.

## Features

- **Blockchain Data Fetching**: Retrieves block and transaction data from `blockchain.info` API.
- **Private Key Recovery**: Exploits the vulnerability of reused `r` values in ECDSA signatures to calculate the corresponding private key.
- **Balance Check**: Verifies if the recovered private key has any Bitcoin balance or has ever received any transactions.
- **Address Formats**: Supports both compressed and uncompressed address formats.
- **Error Handling**: Skips transactions or blocks that are not processed correctly due to errors.

## Prerequisites

- Python 3.x
- `ecdsa` library for elliptic curve operations: `pip install ecdsa`
- Internet connection to fetch data from `blockchain.info`

## How It Works

1. **Block Hash Retrieval**: The script fetches the block hash of a specific block height using the `get_block_hash()` function.
2. **Transaction IDs**: It then retrieves all transaction IDs in the block with `get_txids_from_block()`.
3. **Transaction Parsing**: For each transaction, the script fetches the raw transaction data using `get_rawtx_from_blockchain()` and parses it with `parseTx()` function.
4. **Signature Splitting**: The script splits the ECDSA signature into `r` and `s` components using `split_sig_pieces()` and checks if two inputs in the transaction reuse the same `r` value.
5. **Private Key Calculation**: If `r` values are reused, the script calculates the private key using the `calculate_private_key()` function.
6. **Balance Check**: The script checks the balance of the Bitcoin address associated with the recovered private key using `check_balance()`.
7. **Result Saving**: If the address has any balance or transaction history, the result is saved in the `winning_addresses.txt` file.
8. **Loop**: The script continues to the next block until stopped.

## Usage

1. **Run the script**: Execute the script in the terminal or command line:

   ```bash
   python scanrsz.py
   ```

2. **Input Block Height**: The script will prompt you to enter a block number (height) from which it will start processing.

3. **Output**: If a vulnerable transaction is found, the corresponding private key, address, and balance details will be printed on the console and saved in `winning_addresses.txt`.

## Example Output

```
Enter block number: 700000
Found 2 transactions in block 700000
Found matching r values for txid e3a1b5...
Private Key: 0xabcdef...
Compressed Address: 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa has balance: 100000000, total received: 500000000
```

## Note

- **Timeout**: If the script cannot fetch data due to connection issues, it will terminate with an error message.
- **Vulnerable Transactions**: The script only processes transactions with exactly two inputs and where the `r` value is reused, which is uncommon but still possible in some cases.
- **Error Handling**: All operations are enclosed in `try-except` blocks to handle errors gracefully and continue execution.

