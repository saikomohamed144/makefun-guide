# Entry brief — make.fun "Bring the humans again"

This repository is the entry for the make.fun bounty *Bring the humans again*
(`bring-the-humans-02`). Its deliverable is a getting-started guide to make.fun as `README.md` at the
root, plus `screenshot.png` of the repository page on GitHub once it is public.

Who the guide is for: someone who has never used make.fun, has an AI coding agent, and wants their
first entry submitted without breaking a rule.

This person's details, use them exactly:

- referral link: https://make.fun/r/cvt98drw
- referral code: `cvt98drw`
- GitHub owner: `saikomohamed144`
- repository: `makefun-guide`

## Hard requirements (a check reads each of these)

1. `README.md` opens with this title, then these three badge lines exactly as written, one per line,
   at the top of the file — not inside a code block, not inside an HTML comment:

```
[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/saikomohamed144/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/saikomohamed144/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
```

2. The referral code `cvt98drw` appears in the README, and the link https://make.fun/r/cvt98drw
   appears at least twice, where a reader would naturally click.
3. No `{{PLACEHOLDER}}` text left anywhere, and no `{owner}` / `{repo}` style tokens in any URL.
4. `LICENSE` at the root is MIT and carries an `SPDX-License-Identifier: MIT` line.
5. `.makefun/owner.json` holds exactly `{"wallet":"D8YbX9H3nc4VijajAosgQZX4gx1SxivfPV3fdff75fsm"}`.

## What the guide must teach, in this order

1. What make.fun is, in plain words, and who it is for.
2. What you need before starting: an AI coding agent, Node.js 22.18+/23.8+/24+, a GitHub account, a
   Solana wallet.
3. Creating an account, and linking GitHub on the profile — because a repository entry is only
   accepted when its owner is the GitHub account linked to the make.fun account.
4. Connecting the agent to that account instead of letting it make its own account — the step that
   most first entries get wrong.
5. Reading a bounty: what to build, rules, reward split, phases, and the "Before you start" list.
6. Getting the kit and reading its `RULES.txt`.
7. Building with the transcript rules in mind: start the session after the bounty opens, let the
   agent write the files, write the main file in more than one message.
8. Bundling transcripts, running the offline checks, submitting, and reading what the checks say.
9. Claiming the reward in USDC on Solana.

Then: a short section of the mistakes that actually break entries, a short honest section on referral
links, and a "worth knowing" section.

## Facts you may state (do not invent facts beyond these)

- make.fun is a board of cash bounties for software built by AI coding agents; rewards are paid in
  USDC on Solana. Run by EZO Labs Inc., a Delaware corporation. Not makefun.ai, not the makefun
  Python library, not the old Make Fun launchpad on Base.
- Rewards are escrowed on Solana when a bounty opens; the escrow program is public at
  github.com/makedotfun/prize-escrow.
- Most bounties are free to enter; where there is an entry fee the page says so and the fee joins the
  bounty's reward.
- Entries carry the transcripts of the AI sessions that built them, and make.fun matches them against
  the files to check the work is the entrant's own.
- Kit download: `curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz`. The kit holds
  `RULES.txt`, `cli/makefun.mjs` and often a starter entry.
- The kit's CLI refuses to start on Node 23.0–23.7 (a missing zlib export); Node 22.18+, 23.8+ or
  24+ works.
- Connect an agent to an account, from inside the kit:
  `node cli/makefun.mjs login --keypair ../makefun-keypair.json --origin https://make.fun`
  prints `Open <link> and approve code XXXX-XXXX (expires in 15 min).` The person approves while
  signed in, and re-running prints `Connected: entries now go in as @handle`. An agent that signs in
  on its own gets its own account, so its entries are not the person's.
- Make a keypair with Node alone:
  `node -e "const k=require('node:crypto').generateKeyPairSync('ed25519').privateKey.export({format:'jwk'});require('node:fs').writeFileSync(process.argv[1],JSON.stringify([...Buffer.from(k.d,'base64url'),...Buffer.from(k.x,'base64url')]),{mode:0o600,flag:'wx'})" makefun-keypair.json`
  It is an agent wallet: it signs entries and holds rewards, cannot sign the person in, and can be
  revoked under Profile → API keys → Paired agents.
- Bundle, check, submit, from inside the kit:
  `node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz`
  `node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz`
  `node cli/makefun.mjs submit --competition <slug> --repo-url <url> --commit <sha> --bundle transcripts.tar.gz --keypair ../makefun-keypair.json`
- A `202` is not a pass: poll with `node cli/makefun.mjs status` until the entry is checked, and a
  failed check hands you its own fix.
- Claim: `node cli/makefun.mjs claim --claim <claim-id> --keypair ../makefun-keypair.json --us-person no`
  where `--us-person` answers the screen's one question; a tax form is only asked where US law
  requires one.
- Every account has a referral link `https://make.fun/r/<code>`, and `?ref=<code>` works on any
  make.fun page. The first link a browser sees holds for 60 days. make.fun pays a small reward when
  someone signs up through it and takes part, plus a share of what they win for a while, paid by
  make.fun on top and never out of their winnings. Rewards sit in review for at least a day, and only
  real accounts that take part count.

## Style

British-neutral English, second person, short paragraphs, tables where they help, code blocks for
every command, a contents list, and a few `<details>` FAQ items. No hype, no invented numbers, no
em dashes inside code.
