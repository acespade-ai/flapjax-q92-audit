# First-BNB-funder mapping — FlapJax Q92 recipients

Source: `eth_getBalance` + `eth_getTransactionCount` at latest on a public BSC
node (Binance dataseed), plus the verified funding tx for the one funded
recipient. "Ungroupable" means the address has never received native BNB, so no
BNB funder can be named. No funder was substituted from token transfers or
outbound transactions.

How "never received BNB" is proved: an EOA can only lose BNB by sending a
transaction, which raises its nonce. An address with nonce 0 and balance 0 at
latest therefore never received native BNB. An address with nonce 0 and a
balance equal to exactly one known funding amount received only that funding.

| Recipient | First inbound BNB from | Funding tx | Block | Amount | Note |
|---|---|---|---|---|---|
| 0x4644f7b8679bf0b6f22e69408e2272e9f4417b45 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0x3aeae5f742e34f94db19b6c494072957633fa7ca | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0xdf4ff9d2f45f99f11a90b16f277d4b7edddb72a5 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0xa8fe84bdcae37e978511012dfec736b07b00fc55 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0x42c812fc1d6fb10600e99a1518df970f5be67ad6 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0x3ccb7381ad4ea4eaab88c370818505771f998c80 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0x97cd18b8bbb629b17675d868259ea84c0a2a5a9c | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0xa15c58e7008fc7a0331cfcc86a28af99fd21b93c | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0xabc2c3daffa4c1963299da225c25f2aa7d4ddf62 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0xfdf05af8ddce3621f01713e01721d6e9ad702ea8 | — | — | — | — | Nonce 0, balance 0: never received native BNB |
| 0xfb200657f7266157a45dfcc6c09690726daa24e6 | 0x7C34E9e21eE28A49Ff0b84B61774119E6633359f (treasury) | 0xa3d7042983f4f2352b9b8f324414688db10887e2b2b2a125deca9a8bec6624e7 | 126835408 | 0.0005 BNB | First and only inbound BNB; 2026-10-10 13:25:31 UTC; balance still exactly 0.0005 BNB with nonce 0, so no other funding exists; tx re-verified on a public node (from treasury, to recipient, 0.0005 BNB, block 126835408) |

Conclusion: no shared funder group among recipients. One recipient (FB2006) shares its
funder with the treasury itself; the other 10 have no native-BNB funder.
