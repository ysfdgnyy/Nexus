# Nexus Setup Guide

This guide provides step-by-step instructions for deploying a Nexus contract and backing up your files. The process is straightforward and only takes a few minutes. You can complete it on any server.

---

## System Preparation

### Update and Install Required Packages
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install cmake
sudo apt install build-essential
```

### Install Rustup
```bash
# Select option 1 when prompted
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# After installation is complete, run:
. "$HOME/.cargo/env"
```

### Add Rust Target
```bash
rustup target add riscv32i-unknown-none-elf
```

---

## Nexus Tool Installation

### Install Nexus Tool
This step may take some time. If you encounter errors, don’t worry; they are expected.
```bash
cargo install --git https://github.com/nexus-xyz/nexus-zkvm cargo-nexus --tag 'v0.2.3'
```

---

## Create a Nexus Project

### Initialize a New Nexus Project
```bash
cargo nexus new nexus-project
```

### Modify the `main.rs` File
Navigate to the project directory and edit the `main.rs` file:
```bash
cd nexus-project
nano ./src/main.rs
```

Replace the contents of the file with the following code:
```rust
#![no_std]
#![no_main]

fn fib(n: u32) -> u32 {
    match n {
        0 => 0,
        1 => 1,
        _ => fib(n - 1) + fib(n - 2),
    }
}

#[nexus_rt::main]
fn main() {
    let n = 7;
    let result = fib(n);
    assert_eq!(result, 13);
}
```

---

## Run and Verify the Contract

### Run the Contract
```bash
cargo nexus run
cargo nexus run -v
```

### Prove the Contract
Wait for the proving process to complete:
```bash
cargo nexus prove
```

### Verify the Proof
```bash
cargo nexus verify
```

Save the `nexus-proof` file in this directory for future use.

---

## Network CLI Setup

### Install the Nexus CLI
Open a new screen session:
```bash
screen -S nexus
```

Run the following command to install the Nexus CLI. If a new version is available, it will be downloaded:
```bash
curl https://cli.nexus.xyz/ | sh
```

Once the installation is complete, you can detach the screen session with `CTRL+A D`. If the process is successful, you can find the `prover-id` in the `/root/.nexus/` directory.

---

That’s it! You’ve successfully deployed your Nexus contract and set up the environment. Be sure to back up all important files and proofs for safekeeping.
