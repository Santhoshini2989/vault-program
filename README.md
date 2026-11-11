# Solana Anchor Vault

An on-chain Solana vault program built with the Anchor framework. This project
completes the assignment to implement secure **withdraw** and **close** instructions
using Program Derived Addresses (PDAs) and System Program CPIs with PDA signing.

---

## Assignment Overview

The assignment requirement:
> **1. Write the vault program and include the withdraw and close instructions.**

- **`withdraw`**: Safely transfers SOL from the `vault` PDA back to the user
  via CPI with PDA signing (`[b"vault", user, &[bump]]`), verifying that the vault
  remains rent-exempt.
- **`close`**: Sweeps all remaining lamports from the `vault` PDA back to the
  user and closes the `vault_state` PDA account.

*(The `initialize` and `deposit` instructions serve as the foundation).*

---

## Prerequisites

- **Rust**: 1.80+
- **Solana CLI**: 1.18+ / 2.0+
- **Anchor CLI**: 0.30+ / 1.1+

---

## Setup

Ensure your local keypair exists:

```bash
solana-keygen new --outfile ~/.config/solana/id.json
```

---

## Build & Test

Tests use **LiteSVM** for fast in-process VM execution without running a local validator.

### 1. Build the Program

Compile the Solana program into SBF bytecode (`target/deploy/q3_26_vault.so`):

```bash
anchor build
```

> **Note:** `anchor build` must be run before tests because `test_initialize.rs` embeds `q3_26_vault.so` at compile time via `include_bytes!`.

### 2. Run Tests

Execute the integration test suite:

```bash
cargo test
```

*(or run `anchor test`)*

---

## Instructions Summary

| Instruction | Description | Accounts / Signers |
| :--- | :--- | :--- |
| `initialize` | Initializes `VaultState` and funds rent exemption | `user` (Signer), `vault_state`, `vault` |
| `deposit` | Transfers SOL from user to `vault` PDA | `user` (Signer), `vault_state`, `vault` |
| **`withdraw`** *(Implemented)* | Withdraws SOL to user; validates rent exemption; signs via PDA seeds | `user` (Signer), `vault_state`, `vault` (PDA Signer) |
| **`close`** *(Implemented)* | Sweeps all vault lamports to user and closes `vault_state` | `user` (Signer), `vault_state` (closed), `vault` (PDA Signer) |
