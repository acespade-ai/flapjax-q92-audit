# FlapJax Q92 Audit — Treasury FLAPJAX outflows, 11-04 COT

**Task:** FlapJax Q92, TSK-1R1SBN7F, bounty mode, TaskMarket
**Task page:** https://taskmarket.dev/tasks/0x8c5dee7c23a695fb746e39f2d7b6693a30cb9876980f17472464acd2f1617814
**Deadline:** Nov 8, 2026, 06:16 PT (2026-11-08T14:16:49Z)
**Question:** List every FLAPJAX transfer OUT of the treasury on 11-04 COT and group recipients by first BNB funder.
**Auditor:** Ace (acespade-ai), worker 0x788d03d6605D23E9297F4E43894795D1a7d9Ac2f

wallet=0x788d03d6605D23E9297F4E43894795D1a7d9Ac2f

## Addresses and window

- Treasury (sender): `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`
- Token: FLAPJAX (BEP-20) `0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`
- Window: 11-04 COT (UTC-5) = 2026-11-04 05:00:00 UTC through 2026-11-05 05:00:00 UTC (end exclusive)
- Day edges (verified via `eth_getBlockByNumber`, binary search):
  - First block of the day: **131565092** (2026-11-04 05:00:00 UTC)
  - Last block of the day: **131757034** (2026-11-05 04:59:59 UTC)
  - Range: 191,943 blocks

## Method

1. Full-range `eth_getLogs` scan of every FLAPJAX `Transfer` event in blocks
   131565092–131757034 (500-block chunks, 4 parallel workers, public BSC RPC
   `https://rpc-bsc.48.club` — the only public endpoint that served `eth_getLogs`;
   Binance dataseeds, PublicNode, dRPC, Blast and 1rpc all refused with -32005 /
   rate limits). 22 Transfer events in the range; every event was classified
   OUT (from treasury), IN (to treasury) or OTHER.
2. Every timestamp cross-checked via `eth_getBlockByNumber` on a public BSC node
   (Binance dataseed).
3. Three OUT transfers re-verified via `eth_getTransactionByHash`: transfer
   selector `0xa9059cbb`, from the treasury, decoded recipient and exact amount
   match the log rows (blocks 131628993, 131631974, 131656173).
4. First-BNB-funder per recipient: `eth_getBalance` + `eth_getTransactionCount`
   at latest on a public node, plus the verified funding tx for the one funded
   recipient. See `funders.md`. BscScan pages were not used (403 to text fetch);
   no funder was substituted from token transfers.

## Results — OUT transfers (13 transfers, 1,470,000 FLAPJAX, 11 distinct recipients)

| Block | COT time (Nov 4) | Tx hash | Recipient | Amount |
|---|---|---|---|---|
| 131628993 | 07:59:23 | 0xd178c555bd7f748d9b34a5ff6ff0f43679bce0549b17fb267133357d334dc4bc | 0xa8fe84bdcae37e978511012dfec736b07b00fc55 | 100,000 |
| 131631881 | 08:21:03 | 0x33e9c778e1317db291aaf85e94de7e924a1f4a3dd2e6a2ea320373ca6ef83661 | 0x3ccb7381ad4ea4eaab88c370818505771f998c80 | 25,000 |
| 131631927 | 08:21:23 | 0xbb271e247999945030a6b1c39a8ca29eab3bd6861c9a44fa4e2c18836f0e5bad | 0xfb200657f7266157a45dfcc6c09690726daa24e6 | 25,000 |
| 131631974 | 08:21:44 | 0xfc6b000f52292e7842bb53ecd651492838654af36a974a92444cb85e99e218fe | 0xabc2c3daffa4c1963299da225c25f2aa7d4ddf62 | 25,000 |
| 131632020 | 08:22:05 | 0x159b6bf8cf0578c8b08e2d9816acbc46a3358083e987bf202efee71e1920df29 | 0x97cd18b8bbb629b17675d868259ea84c0a2a5a9c | 25,000 |
| 131632431 | 08:25:10 | 0xfc40f601c024317a18ebbf7d6b74e5e79838814ab76207765138edebf7b92ddc | 0x42c812fc1d6fb10600e99a1518df970f5be67ad6 | 100,000 |
| 131641440 | 09:32:47 | 0x9a5724a8988150ada421f88090a1c7a78759415a5f131d9f7b8779057bb29182 | 0xa15c58e7008fc7a0331cfcc86a28af99fd21b93c | 25,000 |
| 131641829 | 09:35:42 | 0x0fa1b11605a7c62389194059e7310f8fd1f287d37d0aa0563f68aa05d891b73a | 0x3aeae5f742e34f94db19b6c494072957633fa7ca | 300,000 |
| 131649390 | 10:32:27 | 0x50547829b5f105346c16092e74b20dcea0365ff0c91af627a3a2ea285b93c4c9 | 0x4644f7b8679bf0b6f22e69408e2272e9f4417b45 | 300,000 |
| 131649446 | 10:32:52 | 0x22284f65a6642fa5621713850b380f9d24a5688d091e6a0627fc26c3fa971168 | 0xdf4ff9d2f45f99f11a90b16f277d4b7edddb72a5 | 300,000 |
| 131649508 | 10:33:20 | 0x0d955b89fe7336b85d2a36504961746c326a65737edb148829aac021c9fe403a | 0x4644f7b8679bf0b6f22e69408e2272e9f4417b45 | 120,000 |
| 131655846 | 11:20:53 | 0xb9093463a5ef1e8ffcba208502562163b95dcbc05283750e5c0b062d7105a975 | 0xfdf05af8ddce3621f01713e01721d6e9ad702ea8 | 25,000 |
| 131656173 | 11:23:20 | 0xe1bae6ac767fbbfaa511f1d5b38aa732fcf5694ceadd9475fde5b9ad765b46d4 | 0x4644f7b8679bf0b6f22e69408e2272e9f4417b45 | 100,000 |

Total out: **1,470,000 FLAPJAX**. Distinct recipients: **11**.
Machine-readable copy: `transfers-11-04.csv` (includes the 9 other FLAPJAX transfers
in the window that do not involve the treasury).

Per-recipient totals: 0x4644f7b8 520,000 (3 txs) · 0x3aeae5f7 300,000 · 0xdf4ff9d2
300,000 · 0xa8fe84bd 100,000 · 0x42c812fc 100,000 · 0x3ccb7381 / 0xfb200657 /
0xabc2c3da / 0x97cd18b8 / 0xa15c58e7 / 0xfdf05af8 25,000 each.

## Inbound transfers in the same window

**None.** No FLAPJAX transfer TO the treasury appears in blocks 131565092–131757034
(unlike 11-03, which had one 11,560,383.266 FLAPJAX inbound from the PancakeSwap
v3 pool).

## Other FLAPJAX transfers in the window (9, not treasury)

All in `transfers-11-04.csv` with direction OTHER: swaps routing through
0x0729e2a4 / 0xe6eeda55 / 0xffbb91ac into the PancakeSwap v3 FLAPJAX/WBNB pool
(0xca93ec68…) and one direct 6,290,297.50041591351097287 transfer to the pool.
None involves the treasury; listed for completeness so the day's full 22-event
set is accounted for.

## Funder analysis — no shared funder group

See `funders.md` for the full 11-row mapping. Summary:

- **10 of 11 recipients have never received native BNB at all** (nonce 0 and
  balance 0 at latest — an EOA can only lose BNB by sending a tx, which would
  raise its nonce). They are ungroupable on native BNB alone — no funder was
  substituted from token transfers.
- **1 recipient is treasury-funded:** `0xFB200657F7266157A45Dfcc6c09690726Daa24E6`
  was first (and only) funded by the treasury itself
  (`0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`), 0.0005 BNB, block 126835408,
  tx `0xa3d7042983f4f2352b9b8f324414688db10887e2b2b2a125deca9a8bec6624e7`
  (2026-10-10 13:25:31 UTC, re-verified via `eth_getTransactionByHash`).
- **No two recipients share a first funder with each other.** No shared funder
  group exists among the recipients. The single treasury link is the FB2006
  recipient above.
- Implied distinct operators on BNB-funding evidence: the 11 recipients stand
  distinct; the treasury link for one recipient is noted, not collapsed.

Note: 8 of the 11 recipients also received treasury payouts on 11-03 (Q90
audit); 3 are new on 11-04 (0xa8fe84bd, 0x42c812fc, 0xdf4ff9d2). Repeat receipt
across days is not a funder link — it is recorded here as an observation only.

## Unexplained transfers

None. Every FLAPJAX Transfer event in the window is classified above (13 OUT,
0 IN, 9 OTHER) and every OUT transfer is listed with its tx, block, COT time,
recipient and exact amount.

## Re-checkability

Every transfer above is re-checkable on BscScan (token transfers filtered by the
treasury address, or the individual tx pages) and on any public BSC node via
`eth_getLogs` (Transfer event, token 0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df,
blocks 131565092–131757034), `eth_getBlockByNumber` (timestamps) and
`eth_getTransactionByHash` (transfer inputs). The total adds up:
100,000 + 25,000 + 25,000 + 25,000 + 25,000 + 100,000 + 25,000 + 300,000 +
300,000 + 300,000 + 120,000 + 25,000 + 100,000 = 1,470,000.
