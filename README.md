# kevins-challenge
My work env is basically an MSYS2 cli (Arch linux env on windows). 
This is a concise quick summary. Refer to `tutorial.md` for the detailed guide and some uncecessary philosophy on each step (Coming Soon)
All raw files are here for you to test them if you wish. 

## Step 1: Creating a rust project (main.rs)

`cargo new <proj-name>`

You'll find the main.rs in side the src folder. I've put a simple addition-subtraction program in there for testing. 

## Step 2: Rust to LLVM 

`rustc --emit=llvm-ir main.rs`

You'll get a main.ll file in the same directory as the main.rs file. 

## Step 3: (Optional) LLVM optimisations 
because optimising in terms of size (-Oz level) will help have smaller packages for faster shipping, validation, lower gas-fee, better interoperability etc.
we'll be using `opt` for optimizing the llvm file.

`opt -Oz main.ll -o main_optimized.ll`

You'll find the optimized file in the same src directory.
note the drastic change in decreasing of file sizes. 

## Step 4: LLVM to wasm object
going from llvm to wasm is a two-part step: llvm -> object file -> wasm package
we'll be using `llc` for this step.

`llc -march=wasm32 -filetype=obj main_optimized.ll -o main.o`

you will get a main.o wasm object file in the same directory (you can also get wasm-text using the same llc command)

## Step 5: wasm object to .WASM
finally, to get our wasm package, we will use a linker (wasm-ld)

`wasm-ld --no-entry --export-dynamic --allow-undefined main.o -o main.wasm`

This command tells `wasm-ld` not to expect a main function (`--no-entry`), to export all symbols (`--export-dynamic`), to allow undefined symbols (`--allow-undefined`), and to output the result to `main.wasm` in the same src directory.


