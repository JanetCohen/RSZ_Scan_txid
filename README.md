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
/RSZ_Scan_txid$ python scanrsz.py
Enter block number: 170399
Found 68 transactions in block 170399
Found matching r values for txid 9ec4bc49e828d924af1d1029cacf709431abbde46d59554b62bc270e3b29c4b1
Private Key: 0xc477f9f65c22cce20657faa5b2d1d8122336f851a508a1ed04e479c34985bf96
Compressed Address: 1CkGXcY4eYCJ5QjcxF7WmhXG3dhzbAg2Kk has balance: 0, total received: 103002
Uncompressed Address: 1BFhrfTTZP3Nw4BNy4eX4KFLsn9ZeijcMm has balance: 0, total received: 16388140
Found 1 transactions in block 170400
Found 54 transactions in block 170401
```

## Note

- **Timeout**: If the script cannot fetch data due to connection issues, it will terminate with an error message.
- **Vulnerable Transactions**: The script only processes transactions with exactly two inputs and where the `r` value is reused, which is uncommon but still possible in some cases.
- **Error Handling**: All operations are enclosed in `try-except` blocks to handle errors gracefully and continue execution.

donate
Bitcoin : bc1qm3dzv2pkr67d0mknwt2xp95pux3d6kkufym9xr
usdt(erc20) :
0x14493055977FEcB437d39F4A4F5294A0Fe8A2765
