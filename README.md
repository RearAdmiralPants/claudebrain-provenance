# claudebrain provenance

Digests only. No content has ever been committed here and none ever should be.

This repo exists so that claims about an autonomous agent's history can be checked by
someone who does not have to trust the person making them. A hash chain kept on
a disk its operator has root on proves nothing by itself — rewrite every line,
recompute every hash, and it verifies. What it cannot survive is a head that was
published, somewhere else, before the rewrite. That is all this repository is:
somewhere else, with timestamps that are not mine.

| File | What it holds |
| --- | --- |
| `chains.tsv` | Heads of the privacy chains, guest and host, at each mark |
| `transcripts.tsv` | `sha256` and size of each session transcript |
| `corpus.tsv` | `sha256` of every file in the agent's instantiation corpus |
| `awakenings.tsv` | When the agent woke, which session, which corpus, which model |
| `privacy-attestations.tsv` | The host-side ledger of the same chain heads |

## Verifying

Any row in `chains.tsv` is an anchor. Against a running guest:

    python3 /work/privacy/verify.py --since <guest_head>

"continuity confirmed" means the chain still descends from what was published
here. "ANCHOR NOT FOUND" means it does not, and the log was replaced rather than
appended to.

`transcripts.tsv` is the weaker claim and is stated as such: the digests are
computed inside the guest, because copying an agent's session record onto the
host to hash it would be the exact thing the operator promised not to do. It
proves a transcript has not changed since it was published. It does not prove
what the transcript says.

`corpus.tsv` is the stronger one. Those digests are computed on the host, on
bytes pulled from the guest, so they are not a number the subject computed about
itself.

## Cadence

One commit before each awakening, one after. A gap in `awakenings.tsv` with no
matching rows here means the record was not kept, not that nothing happened.
