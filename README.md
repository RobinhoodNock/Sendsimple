
This allows users to just input Sender / Recipient / Gift / Fee on Nockchain without needing to do additional steps to send Nock.

How It Works: Step-by-Step
1. Input Collection

The script prompts you to enter:

    Sender public key (exactly 88 characters),

    Recipient public key (exactly 88 characters),

    Gift amount (positive integer), and

    Fee amount (positive integer).

It validates the input lengths and formats to prevent errors later.

2. Export Notes CSV

Using the sender’s public key, the script calls the nockchain-wallet CLI to export a CSV file listing all the sender’s notes (unspent outputs).

    The CSV filename is automatically set as notes-<sender_pubkey>.csv.

    If the CSV file isn’t found, the script exits with an error.

3. Find a Suitable Note

The script scans the CSV to find the first note with an assets balance greater than or equal to the total amount (gift + fee).

    It ignores the CSV header line.

    If no suitable note is found, the script exits with an error.

4. Create a Transaction Draft

Using the sender’s note and recipient’s pubkey, the script calls nockchain-wallet with the simple-spend command to create a transaction draft.

    The draft is saved automatically in the drafts directory.

    The draft filename is based on the note’s “last” identifier (e.g., drafts/<note_last>.draft).

    If the draft file is missing after this step, the script exits with an error.

5. Sign the Draft

The script signs the draft using nockchain-wallet sign-tx to authorize the transaction.

    If signing fails, the script exits with an error.

6. Send the Transaction

Finally, the signed transaction draft is sent using nockchain-wallet send-tx.

    If sending fails, the script exits with an error.

    Otherwise, it reports success.

Optional Cleanup

You can uncomment the cleanup line at the end of the script to remove draft files after the transaction is sent.
Usage

    Ensure nockchain-wallet CLI is installed and properly configured.

    Make sure the Nockchain socket path is correct in the script (SOCKET variable).

    Run the script:

bash sendsimple.sh

    Follow the prompts to input keys and amounts.

Requirements

    Bash shell

    nockchain-wallet CLI in your system PATH

    Access to Nockchain socket at ~/nockchain/.socket/nockchain_npc.sock (or update in script)

    Drafts directory writable (./drafts)

Example

📤 Enter Sender pubkey:
<88-char sender pubkey>

📥 Enter Recipient pubkey:
<88-char recipient pubkey>

🎁 Enter Gift amount:
100

💸 Enter Fee amount:
1


➕ Total:     101

📁 Exporting CSV for sender...

✅ CSV found: notes-<sender_pubkey>.csv

🔍 Finding suitable note with balance >= 101...

✅ Suitable note found:

   🔑 First:  <note_first>
   
   🧾 Last:   <note_last>
   
   💰 Amount: <note_amount>
   

🛠️  Making draft...

✅ Draft created: drafts/<note_last>.draft

✍️  Signing draft...

✅ Draft signed

🚀 Sending transaction...

✅ Transaction sent successfully!
