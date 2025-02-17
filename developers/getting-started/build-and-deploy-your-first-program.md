# Build & Deploy Your First Program

This guide provides a step-by-step overview of deploying a program on Sonic SVM. Familiarity with command-line tools is necessary.

## **1. Install CLI Tools**

Ensure that the Solana command line tools are installed as you will be using these tools throughout the deployment process.

## **2. Write Your Program**

Develop your program logic. For those using Rust with Anchor, input your program logic into the `lib.rs` file located in the `src` directory of your project. Confirm that your program compiles correctly and passes all tests.

## **3. Build Your Program**

Compile your program to a BPF (Berkeley Packet Filter) executable, the format required for on-chain programs:

### **With Anchor**

Run `anchor build`. [Anchor](https://www.anchor-lang.com/) manages the compilation details.

### **Without Anchor**

Use `cargo build-bpf` for direct Rust usage.

## **4. Deploy Your Program**

Before deploying your program, ensure your CLI is set to the [Sonic Devnet](../../architecture/clusters.md#devnet) network using the following command:

```bash
solana config set --url https://devnet.sonic.game
```

After deploying your building your program and configuring your RPC URL, you can deploy your program with the following command:

```
solana program deploy <PATH_TO_YOUR_COMPILED_PROGRAM>
```

Ensure that you have enough SOL in your wallet to cover deployment costs. You can get some devnet tokens from the [Sonic faucet](https://faucet.sonic.game).

***

After configuring your network, you may now deploy your program to Sonic. Use the following commands based on your setup:

### **With Anchor**

If the Anchor CLI is installed, you can deploy your program with the following command. This handles the deployment automatically.

```bash
anchor deploy
```

### **Without Anchor**

If you're using the native solana method to author your program, you can also use the Solana CLI to deploy your program.

## **5. Verify Deployment**

Post-deployment, check that your program operates as intended. Interact with your program using Solana's web3.js library or the CLI tools to send transactions.

### **Additional Tips:**

* Keep your program's keypair secure for future upgrades or interactions.
* Monitor the network and your program's performance, particularly if it gains widespread usage.

## **Example: "Hello Sonic!" Program**

This example implements a simple Greeting Counter Program on Sonic.

### With Solana Native Program

```rust
use borsh::{BorshDeserialize, BorshSerialize};
use solana_program::{
    account_info::{next_account_info, AccountInfo},
    entrypoint,
    entrypoint::ProgramResult,
    msg,
    program_error::ProgramError,
    pubkey::Pubkey,
};

#[derive(BorshSerialize, BorshDeserialize, Debug)]
pub struct GreetingAccount {
    pub counter: u32,
}

entrypoint!(process_instruction);

pub fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    _instruction_data: &[u8],
) -> ProgramResult {
    
    msg!("Hello, Sonic World!");
    
    let accounts_iter = &mut accounts.iter();
    let account = next_account_info(accounts_iter)?;

    if account.owner != program_id {
        msg!("Greeted account does not have the correct program id");
        return Err(ProgramError::IncorrectProgramId);
    }

    let mut greeting_account = GreetingAccount::try_from_slice(&account.data.borrow())?;
    greeting_account.counter += 1;
    greeting_account.serialize(&mut *account.data.borrow_mut())?;
    
    msg!("Greeted {} time(s)!", greeting_account.counter);
    Ok(())
}
```

### With Anchor Program

If you are building with [**Anchor**](https://anchor-lang.com), you may use the variant below. Please remember to change the program ID after building your program with `anchor build` before deploying it.

```rust
use anchor_lang::prelude::*;

// !!! Replace with your program ID after running `anchor build` !!!
declare_id!("<REPLACE_WITH_YOUR_PROGRAM_ID>");

#[program]
pub mod hello_sonic_world {
    use super::*;

    pub fn initialize(ctx: Context<Initialize>, authority: Pubkey) -> Result<()> {
        let greeting_account = &mut ctx.accounts.greeting_account;
        greeting_account.counter = 0;
        greeting_account.authority = authority;
        Ok(())
    }

    pub fn increment_greeting(ctx: Context<IncrementGreeting>) -> Result<()> {
        msg!("Hello, Sonic World!");

        let greeting_account = &mut ctx.accounts.greeting_account;
        greeting_account.counter += 1;

        msg!("Greeted {} time(s)!", greeting_account.counter);
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = user, space = 8 + 4 + 32)]
    pub greeting_account: Account<'info, GreetingAccount>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct IncrementGreeting<'info> {
    #[account(mut, has_one = authority)]
    pub greeting_account: Account<'info, GreetingAccount>,
    pub authority: Signer<'info>,
}

#[account]
pub struct GreetingAccount {
    pub counter: u32,
    pub authority: Pubkey,
}

```

## **Run your dAPP on Sonic Devnet**

Example "Hello World" application in TypeScript:

### With Solana Native Program dApp

```typescript
import {
  Connection,
  PublicKey,
  Keypair,
  Transaction,
  TransactionInstruction,
  SystemProgram,
  sendAndConfirmTransaction
} from '@solana/web3.js';
import bs58 from 'bs58';
import * as borsh from 'borsh';

class GreetingAccount {
  counter = 0;
  constructor(fields?: { counter: number }) {
    if (fields) {
      this.counter = fields.counter;
    }
  }
}

const GreetingSchema = new Map([
  [GreetingAccount, { kind: 'struct', fields: [['counter', 'u32']] }]
]);

const programId = new PublicKey('<REPLACE_WITH_YOUR_PROGRAM_ID>');
const connection = new Connection('https://devnet.sonic.game', 'confirmed');
const feePayer = Keypair.fromSecretKey(
  bs58.decode(
    '<REPLACE_WITH_YOUR_PRIVATE_KEY>'
  )
);

async function sayHello() {
  const GREETING_SIZE = borsh.serialize(
    GreetingSchema,
    new GreetingAccount()
  ).length;

  const greetedAccountKeypair = new Keypair();
  const greetedAccountPubkey = greetedAccountKeypair.publicKey;

  const lamports =
    await connection.getMinimumBalanceForRentExemption(GREETING_SIZE);

  const createGreetingAccountIx = SystemProgram.createAccount({
    fromPubkey: feePayer.publicKey,
    lamports,
    newAccountPubkey: greetedAccountPubkey,
    programId: programId,
    space: GREETING_SIZE
  });

  // Create greet instruction
  const greetIx = new TransactionInstruction({
    keys: [
      {
        pubkey: greetedAccountPubkey,
        isSigner: false,
        isWritable: true
      }
    ],
    programId
  });

  const transaction = new Transaction().add(createGreetingAccountIx, greetIx);

  const txHash = await sendAndConfirmTransaction(connection, transaction, [
    feePayer,
    greetedAccountKeypair
  ]);
  console.log(`Use 'solana confirm -v ${txHash}' to see the logs`);

  const greetingAccount = await connection.getAccountInfo(greetedAccountPubkey);
  // Deserialize the account data
  const deserializedAccountData = borsh.deserialize(
    GreetingSchema,
    GreetingAccount,
    greetingAccount!.data
  );

  console.log(
    'Sonic Greeting successful. Account data:',
    deserializedAccountData
  );
}

sayHello()
  .then(() => console.log('Done'))
  .catch(console.error);
```

### With Anchor Program dApp

```rust
import {
  Connection,
  PublicKey,
  Keypair,
  Transaction,
  SystemProgram,
  sendAndConfirmTransaction
} from '@solana/web3.js';
import { Program, AnchorProvider, web3 } from '@project-serum/anchor';
import { IDL } from './idl/hello_sonic_world.json'; // Make sure you have the correct IDL file

const programId = new PublicKey('<REPLACE_WITH_YOUR_PROGRAM_ID>');
const connection = new Connection('https://devnet.solana.com', 'confirmed');
const feePayer = Keypair.fromSecretKey(
  bs58.decode('<REPLACE_WITH_YOUR_PRIVATE_KEY>')
);

// Anchor setup
const wallet = new AnchorProvider(connection, feePayer, {
  preflightCommitment: 'confirmed',
});

const program = new Program(IDL, programId, wallet);

async function sayHello() {
  // Create the greeting account
  const greetedAccountKeypair = Keypair.generate();
  const greetedAccountPubkey = greetedAccountKeypair.publicKey;

  const lamports = await connection.getMinimumBalanceForRentExemption(
    8 + 4 + 32 // space for the account (discriminator, counter, and authority)
  );

  const createGreetingAccountIx = SystemProgram.createAccount({
    fromPubkey: feePayer.publicKey,
    lamports,
    newAccountPubkey: greetedAccountPubkey,
    programId: program.programId,
    space: 8 + 4 + 32, // size of the GreetingAccount (8 discriminator + 4 counter + 32 authority)
  });

  const transaction = new Transaction().add(createGreetingAccountIx);

  await sendAndConfirmTransaction(connection, transaction, [
    feePayer,
    greetedAccountKeypair,
  ]);

  console.log(`Greeting account created with public key: ${greetedAccountPubkey.toBase58()}`);

  // Initialize the greeting account
  await program.rpc.initialize(feePayer.publicKey, {
    accounts: {
      greetingAccount: greetedAccountPubkey,
      user: feePayer.publicKey,
      systemProgram: SystemProgram.programId,
    },
    signers: [greetedAccountKeypair, feePayer],
  });

  console.log('Greeting account initialized');

  // Increment the greeting counter
  await program.rpc.incrementGreeting({
    accounts: {
      greetingAccount: greetedAccountPubkey,
      authority: feePayer.publicKey,
    },
  });

  console.log('Greeting incremented');

  // Fetch the account data
  const account = await program.account.greetingAccount.fetch(
    greetedAccountPubkey
  );

  console.log('Greeting successful. Account data:', account);
}

sayHello()
  .then(() => console.log('Done'))
  .catch(console.error);
```

***

Running the above script \[either with Solana Native, or with Anchor] programs should output the following:

```bash
Use 'solana confirm -v TsCwj8s8meQFSfqUKdHqDQVzfUbqRcugP2gJ5kaGCj4v4D7aKvohqQQncvUFunqZVH9sS6jhESMjwe6SHGPfrXf' to see the logs
Sonic Greeting successful. Account data: GreetingAccount { counter: 1 }
Done
```

You can find an example of the executed transaction [here](https://explorer.sonic.game/tx/TsCwj8s8meQFSfqUKdHqDQVzfUbqRcugP2gJ5kaGCj4v4D7aKvohqQQncvUFunqZVH9sS6jhESMjwe6SHGPfrXf). The program address for this program is [here](https://explorer.sonic.game/address/BoiBFQbz4ux1rHvAhXEFgujCS57QLby6HxhvBMCp3vkp).
