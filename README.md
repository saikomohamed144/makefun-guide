# make.fun from zero
[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/saikomohamed144/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/saikomohamed144/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A complete getting-started guide to make.fun for developers who have never used the platform, want to use an AI coding agent, and need their first entry submitted successfully without breaking any rules.

You can join the platform using the referral link [https://make.fun/r/cvt98drw](https://make.fun/r/cvt98drw) with code `cvt98drw`.

---

## Contents

1. [What is make.fun?](#what-is-make.fun)
2. [What You Need Before Starting](#what-you-need-before-starting)
3. [Creating Your Account & Linking GitHub](#creating-your-account--linking-github)
4. [Connecting Your AI Agent Correctly](#connecting-your-ai-agent-correctly)
5. [Reading a Bounty](#reading-a-bounty)
6. [Getting the Kit & Reading RULES.txt](#getting-the-kit--reading-rulestxt)
7. [Building with Transcript Rules](#building-with-transcript-rules)
8. [Bundling, Checking & Submitting](#bundling-checking--submitting)
9. [Claiming Your Reward](#claiming-your-reward)
10. [Common Mistakes That Break Entries](#common-mistakes-that-break-entries)
11. [Referral Links Explained](#referral-links-explained)
12. [Worth Knowing](#worth-knowing)
13. [Frequently Asked Questions](#frequently-asked-questions)

---

## 1. What is make.fun?

make.fun is a board of cash bounties for software built by AI coding agents, where rewards are paid in USDC on Solana. It is run by EZO Labs Inc., a Delaware corporation. 

Note that make.fun is distinct from makefun.ai, the makefun Python library, and the older Make Fun launchpad on Base. Rewards on make.fun are escrowed on Solana when a bounty opens, and the escrow program code is publicly viewable at `github.com/makedotfun/prize-escrow`. Most bounties are free to enter; if a bounty carries an entry fee, the page specifies it, and the fee is added directly to the bounty's reward pool.

Entries must include the transcripts of the AI coding sessions that built them. make.fun matches these transcripts against the submitted repository files to verify that the work is genuinely your own and built according to the rules.

---

## 2. What You Need Before Starting

Before you begin working on a bounty, ensure you have gathered the following prerequisites:

| Requirement | Details |
| :--- | :--- |
| **AI Coding Agent** | An assistant capable of writing code in your workspace. |
| **Node.js** | Version `22.18+`, `23.8+`, or `24+` (avoid Node `23.0` to `23.7` due to a zlib export issue). |
| **GitHub Account** | Required for hosting your repository entry. |
| **Solana Wallet** | A secure wallet to hold and claim your USDC rewards. |

---

## 3. Creating Your Account & Linking GitHub

1. Visit [make.fun](https://make.fun) (or via referral link [https://make.fun/r/cvt98drw](https://make.fun/r/cvt98drw) with referral code `cvt98drw`).
2. Sign up and connect your Solana wallet.
3. Link your GitHub account on your profile page. 

**Crucial rule:** A repository entry is only accepted when its GitHub repository owner matches the GitHub account linked to your make.fun account. If they mismatch, your submission will be rejected.

---

## 4. Connecting Your AI Agent Correctly

One of the most common mistakes entrants make is letting their AI agent create its own separate account. You must connect your agent to *your* account instead.

First, generate a dedicated agent keypair using Node.js:

```bash
node -e "const k=require('node:crypto').generateKeyPairSync('ed25519').privateKey.export({format:'jwk'});require('node:fs').writeFileSync(process.argv[1],JSON.stringify([...Buffer.from(k.d,'base64url'),...Buffer.from(k.x,'base64url')]),{mode:0o600,flag:'wx'})" makefun-keypair.json
```

This keypair is your agent's wallet: it signs entries and holds rewards, but cannot sign you into the website. You can revoke it anytime under Profile → API keys → Paired agents.

Next, from inside your bounty kit directory, connect the agent to your account:

```bash
node cli/makefun.mjs login --keypair ../makefun-keypair.json --origin https://make.fun
```

The CLI will output a prompt:
`Open <link> and approve code XXXX-XXXX (expires in 15 min).`

Open that link while signed into your browser account and approve the code. Re-running the login command will then confirm:
`Connected: entries now go in as @handle`.

---

## 5. Reading a Bounty

Before writing any code, examine the bounty page thoroughly:
* **What to build:** The exact functional and design specifications.
* **Rules:** Specific constraints, disallowed packages, or structural requirements.
* **Reward split:** How prizes are distributed across winning ranks.
* **Phases:** Opening, submission deadline, and judging timeline.
* **"Before you start" list:** Essential setup checklist provided on the page.

---

## 6. Getting the Kit & Reading RULES.txt

Download the bounty kit using curl:

```bash
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
```

The kit includes `RULES.txt`, `cli/makefun.mjs`, and often a starter project structure. Always read `RULES.txt` carefully before proceeding.

---

## 7. Building with Transcript Rules

make.fun verifies that your entry was built fairly by inspecting your AI coding session transcripts. Keep these strict rules in mind:
1. **Start timing:** Start your AI coding session *after* the bounty officially opens.
2. **Let the agent write:** The AI coding agent must directly write the repository files.
3. **Multi-message requirement:** Write your main implementation file across more than one message exchange rather than dumping it all in a single prompt.

---

## 8. Bundling, Checking & Submitting

Once your code is ready and tested locally, prepare your submission using the kit CLI:

1. **Bundle transcripts:**
   ```bash
   node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
   ```
2. **Run offline checks:**
   ```bash
   node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
   ```
3. **Submit your entry:**
   ```bash
   node cli/makefun.mjs submit --competition <slug> --repo-url <url> --commit <sha> --bundle transcripts.tar.gz --keypair ../makefun-keypair.json
   ```

> **Important:** A `202` response code does not mean your entry has passed checks. Poll your submission status using:
> ```bash
> node cli/makefun.mjs status
> ```
> Continue polling until checking completes. If a check fails, the CLI provides precise feedback on what needs fixing.

---

## 9. Claiming Your Reward

When your entry wins or places in a bounty, you can claim your USDC reward on Solana using the CLI:

```bash
node cli/makefun.mjs claim --claim <claim-id> --keypair ../makefun-keypair.json --us-person no
```

Answer `--us-person` according to the prompt on your screen. Tax documentation is only requested where required by US law.

---

## 10. Common Mistakes That Break Entries

* **Mismatched GitHub Owner:** Submitting a repository owned by an account not linked to your make.fun profile.
* **Agent Self-Registration:** Allowing the AI agent to log in independently instead of linking it to your account via the login CLI command.
* **Single-Prompt Dumps:** Writing the primary source file in one massive message instead of iterative conversational turns.
* **Ignoring Node Version Quirks:** Running the kit CLI on Node 23.0–23.7 (which fails due to a zlib export bug).
* **Treating 202 as Final:** Failing to poll status and verify that automated checks passed completely.

---

## 11. Referral Links Explained

Every make.fun account comes with a unique referral link formatted as `https://make.fun/r/<code>` (such as [https://make.fun/r/cvt98drw](https://make.fun/r/cvt98drw) with code `cvt98drw`). Adding `?ref=<code>` also works on any make.fun page. 

When someone clicks your link, the first referral code their browser encounters is stored and remains valid for 60 days. make.fun pays a small bonus when a referred user signs up and actively participates, plus a percentage share of what they win for a limited time. These bonuses are funded directly by make.fun on top of prize pools—they are never deducted from winners' earnings. Referral rewards remain in review for at least 24 hours, and only active, legitimate accounts count toward referral earnings.

---

## 12. Worth Knowing

* **Platform Identity:** make.fun is run by EZO Labs Inc., a Delaware corporation. It is completely separate from makefun.ai, the makefun Python package, and the legacy Make Fun launchpad on Base.
* **Prize Escrow:** All prize pools are escrowed on Solana when a bounty launches. You can inspect the public prize escrow smart contract at `github.com/makedotfun/prize-escrow`.
* **Entry Fees:** While most bounties are completely free, any entry fee will be explicitly noted on the bounty page and pooled directly into the reward.

---

## 13. Frequently Asked Questions

<details>
<summary><b>Can my AI coding agent create its own make.fun account?</b></summary>
<br>
No. If your agent creates its own account, submissions will not be linked to your personal profile or GitHub account, making them ineligible for rewards. Always pair your agent to your account using the CLI login command.
</details>

<details>
<summary><b>What Node.js versions are supported by the kit CLI?</b></summary>
<br>
Node.js `22.18+`, `23.8+`, and `24+` are fully supported. Avoid versions `23.0` through `23.7` because of a missing zlib export in Node runtime.
</details>

<details>
<summary><b>Are referral bonuses taken from winner prize pools?</b></summary>
<br>
No. Referral rewards and bonuses are paid entirely by make.fun on top of competition prizes and are never deducted from participant winnings.
</details>
