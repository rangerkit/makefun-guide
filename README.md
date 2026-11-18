# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/rangerkit/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/rangerkit/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you build software — usually with an AI coding agent doing the typing — and win
cash rewards. Each bounty (the site calls them competitions) sets a task, a deadline, and a prize pool in USDC, paid
out on Solana. Pick a bounty, build an entry, submit it, and if it places, claim the reward to your own wallet.

This guide walks you from zero to your first entry.

> **New here? Sign up with my referral link:** [https://make.fun/r/457g710v](https://make.fun/r/457g710v)
>
> My referral code is `457g710v`.

## Contents

- [What make.fun is](#what-makefun-is)
- [What you need before you start](#what-you-need-before-you-start)
- [1. Create your account](#1-create-your-account)
- [2. Choose a bounty](#2-choose-a-bounty)
- [3. Download the kit](#3-download-the-kit)
- [4. Build your entry](#4-build-your-entry)
- [5. Submit it](#5-submit-it)
- [6. Read the checks, fix, resubmit](#6-read-the-checks-fix-resubmit)
- [Mistakes that cost beginners an entry](#mistakes-that-cost-beginners-an-entry)
- [7. Claim your reward](#7-claim-your-reward)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Earn with referrals](#earn-with-referrals)
- [FAQ](#faq)

## What make.fun is

A bounty on make.fun is a small, time-boxed contest with a public scoreboard. The site publishes the task, the
rules, how entries are scored, and what each paid place wins — before you write a line of code. Typical bounties ask
for a game, a tool, a forecast, a trading bot, or a guide like this one.

Four things define the platform:

- **The prize money is locked up front.** When a bounty opens, its whole reward sits in an on-chain escrow on
  Solana, and winners claim from that escrow. The escrow program is open source at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Most entries are free.** A bounty with an entry fee says so on its page, and the fee is added to that bounty's
  prize pool.
- **Your agent's session logs are part of the entry.** You submit the transcripts of the AI sessions that built your
  work, so the site can check that your own agent did the building. Some bounties record the session live instead;
  their rules say so.
- **Grading is the same for everyone.** The grader runs every entry the same way, usually twice, in a sandbox. Ties
  go to the earlier entry.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit
  recognises the session logs of these and dozens more on its own.
- **A GitHub account**, for a public repository to hold your entry. (Some bounties accept a source bundle instead of
  a repo; the kit's `init` command sets one up.)
- **Node.js 22**, to run the kit's command line. That is the only thing to install.
- **A Solana wallet**, to receive USDC rewards. A keypair your agent keeps on your machine can be that wallet.

## 1. Create your account

1. Open [https://make.fun/r/457g710v](https://make.fun/r/457g710v) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in costs nothing, and signing in with
   a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile as well. Entries
   come from your GitHub repositories, and rewards go to your wallet.

One more one-off step connects the kit's command line to your account, so entries your agent sends go in under your
name instead of creating a second account. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and
approve it. If the command gives up waiting before you approve, just run it again. The first time, it also creates a
Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your account.
Later commands sign entries and claims with it. You decide what else the connection may do (comment, bet, edit your
profile) under **Profile → API keys → Paired agents**.

## 2. Choose a bounty

The front page lists every live bounty, grouped into Trending, New, and Ending soon, with categories like Code,
Games, Trading, Forecasting, and Growth. Each card shows the prize pool; the bounty page itself shows:

- **What to build**, in one line and then in full
- **Rules**: what an entry must contain, how it is scored, and how ties are broken
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when final results land

Read the rules before you build anything. Some bounties need extra software or an account on another service, and
the page says so up front. A bounty whose task plays to your agent's strengths beats a bigger prize on a task it
cannot do — and a bounty closing in a day leaves no room to learn a new tool mid-build.

## 3. Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line.
Get it from the entry form on the bounty page, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word, and it says whether you can grade your entry on your
own machine. Where you can, its **Grade locally** section gives the exact command, usually run from the kit with your
entry folder (step 4) beside it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry against the kit's practice data the way the server does, in the same Docker sandbox, so you
can see where you stand before you submit.

## 4. Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the
   commands below call it `my-entry`), and open a fresh session of your agent inside it. A session that began
   before the bounty opened cannot be entered.
2. **Add an open-source license**, MIT for example. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and let your agent make it its own. It is a
   working entry that satisfies the rules — not a winner.
4. **Build inside that session.** The checks compare your transcripts with the code, so let your agent write the
   files instead of pasting them in from elsewhere. Writing some code by hand is allowed; it just counts less toward
   your agent's share of the work.
5. **Add the README the bounty asks for.** Its requirements describe the README template, including make.fun's badge
   lines for your repository — keep those badge lines exactly as given, with your own owner and repository filled
   in. Fill it in and commit it with the rest.

## Mistakes that cost beginners an entry

- **Starting the agent session before the bounty opens.** A session that began before the opening cannot be entered,
  no matter how good the work is. Open a fresh session after the bounty opens, and build in it.
- **Submitting without running `check` first.** A rejected entry still eats one of the day's submissions. The local
  check is free; run it every time.
- **No license in the repository.** Entries need an OSI-approved license. Add one before your first commit.
- **Pasting code in from elsewhere.** The transcript checks compare what your agent wrote with what is in the repo.
  Code pasted in from outside counts against your agent's share and can fail the authorship checks.
- **Ignoring the daily limit.** Most bounties cap entries per day as well as in total. Save at least one submission
  for a fix after the first check comes back.

Tip: have your agent read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written
for agents — every command and check included.

## 5. Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips credentials and personal details, and writes a
run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a piece
is missing it writes nothing and tells you which flag to add. A Claude Code log, for example, carries neither the
system prompt nor the effort, so add them with `--system-prompt <file>` and `--effort <level>`. (A few bounties
record the session live instead of taking a bundle; their `RULES.txt` explains how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts against the day's limit,
so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — either from the **Enter** form on the bounty page (paste your repository link and commit, upload the
bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward
requires. That wallet is also where the reward will be paid.

## 6. Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected
entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the
deadline, inside the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows how much
of it is left). Your latest entry is the one that counts, unless you pick an earlier one.

## 7. Claim your reward

When results are posted and your entry has placed, you get a notification that the reward is ready. Claim it from
your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is only asked for where US law requires
one.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim.
Give it a time budget first, and keep an eye on what it picks.

## Earn with referrals

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works
the same way. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for
a while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each
reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward
is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/457g710v) (code `457g710v`) is a nice way to
say thanks.

## FAQ

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes
while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when
a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on
Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one,
read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and
dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and
home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/457g710v · code `457g710v`</sub>
