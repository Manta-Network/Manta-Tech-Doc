
Subtrate Dev Guide
===================

## 1. Learn Rust Language 
Rust is a modern language that inherit many advanced features from C/C++, OCaml, etc. If you haven't used Rust before, please go through 
[Rust Book (Chapter 1 - 18)](https://doc.rust-lang.org/stable/book/)

## 2. Dev Environment Setup 
[Setup Rust Dev Environment for Substrate](https://substrate.dev/docs/en/knowledgebase/getting-started/)

## 3. Warmup Tutorials
* [Create your first substrate chain](https://substrate.dev/docs/en/tutorials/create-your-first-substrate-chain/)
* [Add a pallet to your runtime](https://substrate.dev/docs/en/tutorials/add-a-pallet/) (Note: this requires using `nighlty-2020-10-06` toolchain, first, `rustup install nightly-2010-10-06`, then, use `SKIP_WASM_BUILD=1 cargo +nightly-2020-10-06 check -p node-template-runtime` instead of the commands in the original tutorial)

## Some extra tips (optional)
* [Build Substrate faster](https://medium.com/commonwealth-labs/build-substrate-in-few-minutes-with-fraction-costs-26fce6aa5066)
