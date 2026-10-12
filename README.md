# snarkjs + circom example

A minimal end-to-end zero-knowledge proof with circom and snarkjs: prove that you know the input behind a Poseidon hash without revealing it, then verify the proof in Node.js. It is a starting point for developers trying Groth16 for the first time.

> **Status:** archived. Built in 2024. Kept for reference, not maintained.

## What it does

- `poseidon_hasher.circom` hashes one private input with circomlib's Poseidon template and exposes the hash as the circuit's only public output.
- `build/` holds the compiled circuit: the R1CS file and the WebAssembly witness generator.
- The Groth16 setup files are committed: the Hermez phase 1 file `powersOfTau28_hez_final_12.ptau`, the proving key `circuit_0000.zkey` and the exported `verification_key.json` (bn128, one public signal).
- `poseidon_hasher_test.js` generates a proof for the input `10` with `snarkjs.groth16.fullProve`, prints the public signals and the proof, and checks it with `snarkjs.groth16.verify`.

## Tech stack

- circom 2 (circuit language)
- circomlib (Poseidon hash circuit)
- snarkjs 0.7 (Groth16 proving and verification on bn128)
- Node.js

## Running it locally

```bash
npm install
node poseidon_hasher_test.js
```

It prints the public signal (the Poseidon hash of `10`) and the proof, then `Verification OK` if the proof checks out.

To rebuild the circuit and keys after changing the circuit, install [circom](https://docs.circom.io/getting-started/installation/) (built with Rust), then:

```bash
circom poseidon_hasher.circom --r1cs --wasm -o build
npx snarkjs groth16 setup build/poseidon_hasher.r1cs powersOfTau28_hez_final_12.ptau circuit_0000.zkey
npx snarkjs zkey export verificationkey circuit_0000.zkey verification_key.json
```

These keys are for learning only. A circuit used in production needs its own phase 2 ceremony.

## License

MIT, see [LICENSE](LICENSE). The dependencies that `npm install` brings in keep their own licenses (snarkjs and circomlib are GPL-3.0).
