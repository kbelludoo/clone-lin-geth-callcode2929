# clone-lin-geth-callcode2929

LIN scalar clone of go-ethereum v1.16.9 `gasCallCodeEIP2929` / `gasDelegateCallEIP2929` / `gasStaticCallEIP2929` (LGPL-3.0 upstream).

This repository holds **only the LIN clone**. Auditable proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open):

- Results: `examples/geth_callcode2929/`
- Harness: `python3 test/prove_geth_callcode2929_external.py`
- Event: `docs/events/EVENT_GETH_CALLCODE2929_CLONE_LIN.rulel`
- Kernel in this repo: [`lin_geth_callcode2929.lin`](lin_geth_callcode2929.lin)

Class: **EXPERIMENTAL** i64-scale. Not a Geth node.

## Named contrast vs CALL

Shared Berlin wrapper (`makeCallVariantGasCallEIP2929`): charge cold-warm `2500` via `UseGas` **before** EIP-150 63/64, then add `2500` back.

| kind | extras before 63/64 |
| --- | --- |
| CALL | memory + 9000 if value + 25000 if value && new |
| CALLCODE | memory + 9000 if value (**never** 25000) |
| DELEGATECALL / STATICCALL | memory only |

Commercial fact: because 63/64 is applied after extras, `CALL − CALLCODE` at value+new is **390**, not 25000. A naive `CALL − 25000` fee estimator is wrong.

Pinned upstream: ethereum/go-ethereum tag `v1.16.9` commit `95665d5703e1023995a0ff93e4ce9eb77e8a59bd` blob `085b018e4c42bf0185da790518ea1d1ee891b682`.
