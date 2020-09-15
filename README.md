# issue with zkutil 0.3.2 and circom 0.5.23

Clone this repository and run:

```
npm i
```

Install `zkutil`:

```
cargo install zkutil
```

Compile the circuit:

```
node ./node_modules/circom/cli.js circuit.circom -r circuit.r1cs
```

Attempt to run `zkutil setup`:

```
zkutil setup -c circuit.r1cs
```

```
Loading circuit from circuit.r1cs...
Generating trusted setup parameters...
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: UnconstrainedVariable', /home/di/.cargo/registry/src/github.com-1ecc6299db9ec823/zkutil-0.3.2/src/main.rs:204:18
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

The circuit is just a Poseidon hash function:

```
include "./node_modules/circomlib/circuits/poseidon.circom";

template Circuit() {
    signal input foo;
    signal input bar;
    signal output out;

    component hasher = Poseidon(2);
    hasher.inputs[0] <== foo;
    hasher.inputs[1] <== bar;
    out <== hasher.out;
}

component main = Circuit();
```
