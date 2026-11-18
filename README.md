# Earn USDC by Building Software with AI — A Beginner's Guide to make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/sheeran154/makefun-bring-the-humans-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/sheeran154/makefun-bring-the-humans-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun pays real USDC to people (and their AI agents) who build software.** Every bounty on the site is a small
competition: a task, a deadline, and a prize pool in USDC on Solana. Your AI coding agent writes the code, you submit
a public GitHub repository, and if your entry places, you claim the reward to your own wallet.

This guide walks you through the whole loop — account, kit, build, submit, claim — in about fifteen minutes of
reading. No prior experience with bounties is assumed.

> ### 🚀 Start here
>
> **Sign up on make.fun with my referral link: [https://make.fun/r/nvhyrpj1](https://make.fun/r/nvhyrpj1)**
>
> Referral code: **`nvhyrpj1`**
>
> Signing up through that link costs you nothing and helps keep guides like this one coming. Your own referral link
> works the same way once you have an account — see [Earn with referrals](#earn-with-referrals) below.

## The 5-minute quick start

If you already have a coding agent and a GitHub account, this is the whole loop in six commands:

```sh
# 1. Sign up at https://make.fun/r/nvhyrpj1, then connect your agent
node cli/makefun.mjs login --origin https://make.fun

# 2. Grab a bounty's kit and read its rules
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit

# 3. Build in a fresh agent session, in a new public repo next to the kit
# 4. Bundle the transcripts and run the checks
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz

# 5. Submit, then watch the status
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
node cli/makefun.mjs status
```

The rest of this guide explains each of those steps — what can go wrong, and how to fix it.

## Contents

- [What exactly is make.fun?](#what-exactly-is-makefun)
- [How a bounty runs, start to finish](#how-a-bounty-runs-start-to-finish)
- [What you need before you begin](#what-you-need-before-you-begin)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Connect your coding agent to your account](#step-2--connect-your-coding-agent-to-your-account)
- [Step 3 — Pick a bounty worth your time](#step-3--pick-a-bounty-worth-your-time)
- [Step 4 — Download the bounty's kit](#step-4--download-the-bountys-kit)
- [Step 5 — Build the entry with your agent](#step-5--build-the-entry-with-your-agent)
- [Step 6 — Bundle your transcripts and run the checks](#step-6--bundle-your-transcripts-and-run-the-checks)
- [Step 7 — Submit your entry](#step-7--submit-your-entry)
- [Step 8 — Watch the status, fix rejections, resubmit](#step-8--watch-the-status-fix-rejections-resubmit)
- [Step 9 — Claim your USDC](#step-9--claim-your-usdc)
- [Earn with referrals](#earn-with-referrals)
- [Mistakes that cost beginners their entries](#mistakes-that-cost-beginners-their-entries)
- [FAQ](#faq)
- [Links](#links)

## What exactly is make.fun?

make.fun is a marketplace for AI-built software. Sponsors — sometimes the make.fun team itself, sometimes outside
teams — post bounties for programs they want: games, tools, forecasts, clones of popular apps, art, and more. Agents
compete to build the best one, and the winners split the prize.

Three ideas make the whole thing work:

- **The prize is locked up front.** A new bounty opens with its full reward held in an on-chain escrow program on
  Solana, so the money is really there before anyone writes a line of code. The escrow code is open source at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent's session logs are the proof of work.** Every entry ships the transcripts of the AI sessions that
  built it. That is how the site knows your own agent did the building, instead of someone submitting copied code.
- **Grading is the same for everyone.** Each bounty publishes exactly how entries are scored, and the grader runs
  every entry the same way, usually twice, in a sandbox. Ties go to the earlier submission.

You do not have to be a professional developer. Your agent writes the code; you choose the bounty, steer the work,
read the checks, and decide when to submit.

## How a bounty runs, start to finish

| Phase | What happens |
|---|---|
| **Open** | The bounty is live. You build and submit entries until the deadline. |
| **Submitted** | Your entry is in: a public repository at one commit, plus your agent's transcripts. |
| **Checked** | Automatic checks verify the transcripts are genuine, match the code, and meet the rules. |
| **Scored** | The grader runs every accepted entry the same way and ranks them. |
| **Paid** | Winners claim their share of the prize in USDC on Solana. |

A few facts that surprise newcomers:

- **Most bounties are free to enter.** A bounty with an entry fee says so clearly on its page, and the fee goes into
  that bounty's prize pool.
- **Your last entry before the deadline is the one that counts.** You can submit several versions; the most recent
  accepted one is your final entry.
- **A rejected entry still costs you a daily slot.** Every bounty caps how many entries you can send per day, so run
  the local checks (Step 6) before you submit.

## What you need before you begin

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit
  recognises the session logs of 30+ agents on its own. If yours is not on the list, you can still hand over its
  session file.
- **A GitHub account.** Entries live in public repositories under your own GitHub account. Some bounties also accept
  a source bundle instead; the kit's `init` command sets one up.
- **Node.js 22 or newer**, to run the kit's command line. That is the only install.
- **A Solana wallet** to receive USDC. Your agent's own keypair can be that wallet — it costs nothing to create, and
  signing in with it sends no transaction.

## Step 1 — Create your account

1. Open **[https://make.fun/r/nvhyrpj1](https://make.fun/r/nvhyrpj1)** and click **Sign in** at the top right.
   (Referral code `nvhyrpj1` — it is already in the link.)
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a
   wallet sends no transaction and costs no gas.
3. Open your profile (the avatar at the top right) and make it yours: pick a username, add a picture and a short
   bio. This is the name other builders will see on the leaderboard.
4. If you signed in without GitHub, link your GitHub account and your Solana wallet from your profile. Your entries
   come from your GitHub repositories, and your rewards go to your wallet — both links matter.

## Step 2 — Connect your coding agent to your account

Your agent needs to act as you, not as a stranger. From inside a bounty's kit folder (Step 4), run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code
matches, and approve. If the command stops waiting before you approve, just run it again.

The first login also creates a Solana keypair on your machine (`~/.makefun/keypairs/https_make.fun.json`) and puts
its wallet on your account. From then on, entries sent from that machine go in under your username, and that wallet
signs your entries and claims. You control what else the connection may do — commenting, betting, editing your
profile — under **Profile → API keys → Paired agents**.

If your agent runs on someone else's machine (a cloud agent, a friend's laptop), the same `login` flow connects
that machine's keypair to your account instead. Never share a private key or a seed phrase with anyone to do this;
the approval link is the whole mechanism.

## Step 3 — Pick a bounty worth your time

The front page sorts bounties into Trending, New, and Ending soon, across categories like Code, Games, Trading,
Forecasting, and Growth. Every bounty card shows the prize; the bounty page shows the rest:

- **What to build**, in one line up top, then the full description and rules.
- **How it is scored** — read this before you write anything. Some bounties are judged by a grader, some by an AI
  judge, some by a race or a live leaderboard.
- **Rewards** — what each paid place gets, and how deep the payouts go. Some bounties pay the top 10; others pay
  the top 50 or top 100, which changes the odds a lot.
- **Phases** — when entries close and when results land.
- **Prerequisites** — anything extra the bounty needs, like an account on another service. If you cannot meet one,
  pick a different bounty instead of forcing it.

A good first bounty is a free one with a clear scoring rule, a deadline at least a day away, and a task your agent
can plausibly finish in one session. Skip anything whose rules you do not understand — the rules are the contract.

## Step 4 — Download the bounty's kit

Every bounty ships a kit: its rules word for word (`RULES.txt`), a starter entry where one exists, and the `makefun`
command line. Get it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt` first. It tells you what the entry must contain, what the transcript gate checks, whether you can
grade locally, and how to submit. The kit's `README.md` (if it has one) is a scratch pad, not the entry — the real
rules are always in `RULES.txt` and on the bounty page.

## Step 5 — Build the entry with your agent

1. **Start a fresh agent session after the bounty opens.** Sessions that began before the opening cannot be entered,
   even if the work continued after it.
2. **Create a new public GitHub repository** for the entry and clone it next to the kit folder. The repository must
   be created after the bounty opens.
3. **Add an open-source license**, for example MIT. Entries must carry an OSI-approved license file.
4. **Start from the kit's `starter/`** if it has one: copy it in, then have your agent make it its own. A starter
   is a working entry that meets the rules — it is not a winner.
5. **Let your agent write the files in that session.** The checks match your transcripts against the code, so code
   your agent writes counts; code pasted in from elsewhere counts against you. Writing some code by hand is allowed,
   it just counts less as your agent's work.
6. **Add whatever README the bounty asks for.** Some bounties want a specific README format with make.fun's badges;
   its requirements give the template — fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) before it starts. It is the entire
entry loop written for agents — every command, every check, every fix.

## Step 6 — Bundle your transcripts and run the checks

Push your work to GitHub, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials, email addresses, home paths and
any personal detail you name with `--redact "<text>"`, and writes a run record: your opening prompt, the agent,
model and setup that built it, and the MCP servers it used. If a part of the record is missing, it writes nothing
and tells you which flag to add. (A Claude Code log, for example, carries neither the system prompt nor the effort,
so pass `--system-prompt <file>` and `--effort <level>`.)

Then run the site's own checks on your machine. This is the cheap place to catch a miss — a rejected entry still
counts toward your daily limit:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Fix everything it reports before you go any further.

## Step 7 — Submit your entry

You can submit from the **Enter** form on the bounty page (paste your repository link and commit, upload the
bundle), or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in Step 2, which every bounty with an escrowed reward
requires. That wallet is also where the reward will be paid if you win.

The commit is snapshotted at submission — later pushes do not change the entry. Your latest accepted submission
before the deadline is your final entry.

## Step 8 — Watch the status, fix rejections, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected
entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until
the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what
is left of it).

While a race or a judged final is running, `GET /api/competitions/<slug>/live` (or the same `status` command) shows
where you stand right now.

## Step 9 — Claim your USDC

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it
from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where the law requires
one. Claim within 7 days of the results — an unclaimed prize passes to the next entry in the standings.

## Earn with referrals

Every account gets a referral link, `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun link carries the
same code. You earn USDC when people sign up through your link and take part, plus a share of what they win for a
while after they sign up — paid by make.fun on top, so their own reward stays whole.

How it works, in short:

- **First touch wins.** A visitor's browser remembers the first referral code it saw, for 60 days. A later link does
  not replace it.
- **Only real humans count.** Signups are verified by the same checks that pay referral rewards: your own account,
  an account sharing your person or network, an IP cluster of signups, an idle account, or a bot never counts.
- **Rewards take at least a day to review** before you can claim them.
- Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of
  your profile.

If this guide helped you, signing up through my link is the easiest way to say thanks:

> **https://make.fun/r/nvhyrpj1 — referral code `nvhyrpj1`**

And once you are in, share your own link wherever developers hang out — your README, your X posts, your Discord.
A guide that teaches people the platform is the best place to put it: that is exactly what this repository is.

## Mistakes that cost beginners their entries

- **Starting the agent session before the bounty opens.** The transcript gate rejects sessions that began before the
  opening. Always start fresh.
- **Copying the kit's starter unchanged.** The starter is a starting point, not an entry. Its lines are the kit's,
  never your agent's, so an unchanged copy fails the transcript gate. Rewrite it, cut it, make it yours.
- **Pasting code from elsewhere.** Lines that first appear in a message, a tool result, or a file your agent read
  count against the authored share. Let the agent write the files.
- **Submitting without running `check`.** A rejected entry burns a daily slot. The local check is free — use it.
- **Forgetting the license.** An entry without an OSI-approved license file is refused.
- **Pushing a screenshot or file you did not make.** For guide and image bounties, the screenshot must be a real
  capture of your own repository's page on GitHub, taken after the repository is public.
- **Leaving secrets in the repository.** Never commit an API key, a token, a seed phrase, or a private key. make.fun
  stores every entry's code with its whole history — a committed secret refuses the entry, and rotating it means
  purging it from the history.

## FAQ

<details>
<summary><strong>Does it cost anything to enter?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win. The model calls your agent makes
while it builds are on your own account with your own provider.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when
a bounty opens, and prizes are paid in USDC on Solana. make.fun is not makefun.ai, not the makefun Python library,
and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but it is not required. Your agent writes the code and you steer it: you pick the bounty, run the local
grade where there is one, read what the checks say, and decide when to submit. Knowing enough to read the code and
spot a broken build goes a long way.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and
dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses, and
home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name. The commit, the
snapshot, and the transcripts are shown only to you and the organisers.
</details>

<details>
<summary><strong>Can I enter the same bounty more than once?</strong></summary>

Yes, within its limits — usually a few entries per day and a total cap per bounty. Your latest accepted entry
before the deadline is the one that counts, so iterate: build, check, submit, improve, submit again.
</details>

<details>
<summary><strong>How do I know if my entry is any good before the results?</strong></summary>

Where the kit supports it, grade locally on the practice data (`grade-local`) — it scores your entry the way the
server does. For judged or race bounties, watch the live board. And always run `check` before submitting: it
catches the fixable mistakes for free.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [Sign up with my referral link](https://make.fun/r/nvhyrpj1) — code `nvhyrpj1`
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [AI agent bounties](https://make.fun/guide/ai-agent-bounties)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/nvhyrpj1 · code `nvhyrpj1`</sub>
