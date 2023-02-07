# plonky2-hashchain

## Run

```
rustup override set nightly
```

```
RUSTFLAGS="-C target-cpu=native -A warnings" cargo test -r -- --nocapture
```

TRIGGER WARNING: author is extremely new to Rust, so pointers and unwraps are flying everywhere, don't search too much logic in it.

## What it does


This prototype generates the proof of the following statement: "there exists such k>0, that Hash^k (x) = y".

k is unknown, and unlimited from above (marginal cost of increasing k by 1 is negligible).

[Here](https://morgana-proofs.github.io/mpc-maci/master/chapter-5-random-ideas/keychain.html) is some motivation why we might want it.

In practice, with ``DEPTH=60`` will mean that it is bounded by 2^70, which I believe should be unreachable.

On my old Lenovo Legion Y520, generation for ``DEPTH=60`` takes roughly 5 minutes + small linear cost depending on k (~6/1024 seconds).

Current test picks k randomly up to 10000, and creates two different proofs, checking that their verifier_data coincides // i.e., they actually do represent the same circuit.

It makes sense to also play with ``BATCH_SIZE``. Currently it is set to be ``1024``, which means that the ground proof is computed much faster than the first recursion. However it seems that increasing it too much also slows down first few recursive proofs of low depth, so it is unclear to me what is the optimal spot.

Current version doesn't use rayon at all because I'm noob and my purpose currently is to just demonstrate feasibility of such an approach.
