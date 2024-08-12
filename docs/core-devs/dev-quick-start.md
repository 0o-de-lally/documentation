# Libra Move Dev Quick Start

## Cheat Sheet
```
# run framework tests with
cd ./framework/libra-framework
libra move test

# run formal verification with
libra move prove

# compile a framework build for Rust tests
libra move framework release
```

## Set up environment

All dev work can be achieved using `libra-framework` repo. We'll need to build some executables from diem and install them on your dev machine.

### Install `libra`
#### If you are mainly working on Move tests.

- You must install `libra` cli tool to your PATH.

```
# in this repo
cargo build --release -p libra


# copy to a dir in your PATH
cp ./target/release/libra ~/.cargo/bin
# you may need to make it executable
chmod +x ~/.cargo/bin/libra
```

### Install `diem-node`
#### If you are also running Rust tests and Smoke Tests.

- You need a standalone `diem-node` binary before working on `libra-framework`
- compile `diem-node` to `$HOME/.cargo/bin`
- Note that the `--profile release` compilation profile makes for much smaller binaries (e.g. `diem-node` goes from about 2GB to 30MB).

```
export RUST_DIEM_COIN_MODULE="libra_coin"
export RUST_DIEM_COIN_NAME="LibraCoin"

cargo build --profile release -p diem-node

# copy to a dir in your PATH
cp ./target/release/diem-node ~/.cargo/bin
# make it executable
chmod +x ~/.cargo/bin/diem-node
```

- export these env vars in your dev env, `~/.bashrc` or `~/.zshrc`

```
export RUST_MIN_STACK=104857600
export DIEM_FORGE_NODE_BIN_PATH="$HOME/.cargo/bin/diem-node"
```



## Running Move unit tests

Change into a Move project dir (i.e., the directory with a Move.toml).

`libra move test`

optionally with filters:

`libra move test -f my-test`

## Build a libra framework release for smoke tests (head.mrb)

```
cd ./framework
cargo run release

```

Your release will be in `./releases/head.mrb`, you will need this for genesis and smoketests.

Note for smoke tests: you must regenerate the .mrb file EVERYTIME YOU MAKE A CHANGE TO CORE MOVE CODE. Otherwise your tests will be against the old code

## Running smoke tests

Do it yourself:
Make sure you are in the root of the project.

Note: there is an issue with the rust default stack size for tests which involve compiling, and then starting a local testnet

```
cd ./smoke-tests
export RUST_MIN_STACK=104857600
export DIEM_FORGE_NODE_BIN_PATH="$HOME/.cargo/bin/diem-node"
cargo test
```

## Troubleshooting

### Can't find `libra`


**Solution**

Check your $PATH.

Instructions assume that you have a `~/.cargo/bin` which is added to your environment's $PATH.


### `disable_lifo_slot`

**Issue**

If you encounter the following error:
`error[E0599]: no method named disable_lifo_slot found for mutable reference &mut tokio::runtime::Builder in the current scope`

**Solution**

You can resolve this issue by building with the following flag:

```
RUSTFLAGS="--cfg tokio_unstable" cargo build --release -p diem-node
```

This flag enables the unstable features required by the tokio runtime.
