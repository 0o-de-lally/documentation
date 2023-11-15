---
title: "Validator Quickstart"
id: "index"
hidden: false
---

## Quick Start
On an Ubuntu 22.04 host:

``` bash
# run all this in a tmux session, a cheatsheet below
tmux

# 1. checkout the source

git clone https://github.com/0LNetworkCommunity/libra-framework.git
cd libra-framework

# 2. Install dependencies and Rust lang

sudo apt update
sudo apt install -y git tmux jq build-essential cmake clang llvm libgmp-dev pkg-config libssl-dev lld libpq-dev
curl https://sh.rustup.rs -sSf | sh -s -- --default-toolchain 1.70.0 -y
. ~/.bashrc

# 3. build and install the binary

cargo build --release -p libra  && cp target/release/libra ~/.cargo/bin

# DONE. check you can run it

libra -v
```

For a more detailed rundown see [here](/validators/running-a-validator)
