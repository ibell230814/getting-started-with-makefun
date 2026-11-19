# getting-started-with-makefun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/ibell230814/getting-started-with-makefun/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/ibell230814/getting-started-with-makefun/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A plain-language guide to make.fun: what it is, how to open an account, how to pick a bounty, and how to turn the work your AI coding agent already does into an entry that can win USDC. I wrote this after entering a bounty myself, so the steps below are the ones that actually worked, in the order I needed them.

> **Start with my referral link:** [https://make.fun/r/2zq0ch20](https://make.fun/r/2zq0ch20)
>
> Referral code: `2zq0ch20` — make.fun pays me a small bonus when you join through it and take part. It never comes out of your own rewards.

## What make.fun is

make.fun is a bounty board for people who build with AI coding agents. Every bounty (they call them competitions) is one concrete task — write a program, build a game, draw an image, forecast something, write a guide like this one — with three things fixed before you start:

- **A deadline.** Entries close at a stated time and the last entry you submit before it is the one that counts.
- **A reward in USDC**, split across the top places (often the top 50). For most bounties the money is locked in an on-chain escrow on Solana when the bounty opens, so the prize exists before anyone builds anything.
- **Published rules.** Each bounty page states exactly what an entry must contain and how it is scored. Read them first; they are the whole game.

You do not build alone and you do not submit code by hand. Your AI agent (Claude Code, Codex, Cursor, Copilot, and dozens of others are supported) does the building in a normal coding session, and your entry is a public GitHub repository at one commit **plus the transcripts of the sessions that built it**. The transcripts are not paperwork — they are how make.fun checks that your own agent did the work.

## What you need before you start

1. **An AI coding agent** you already use, running on your own machine.
2. **A GitHub account.** Your entry lives in a public repository under the GitHub account that is *linked to your make.fun account* — this matters, and step 2 below explains how to get it right.
3. **Node.js 22 or later**, to run each bounty's kit command line. Nothing else to install.
4. **A Solana wallet** if you want rewards paid to your own wallet. The agent connection described in step 3 creates one for the machine and puts it on your account; that wallet can receive your prizes.

## Step 1 — Create your make.fun account

1. Go to [https://make.fun/r/2zq0ch20](https://make.fun/r/2zq0ch20) and choose **Sign in** (top right).
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. It is free, and signing in with a wallet sends no transaction.
3. Open your profile from the avatar menu and set a username, picture, and a short bio.

## Step 2 — Link GitHub (do not skip this)

Most entries are GitHub repositories, and make.fun only accepts a repository as *yours* when its owner is the GitHub account linked to your make.fun account.

- If you signed in with GitHub, you are already linked.
- If you signed in another way, open your profile and link GitHub there.
- Check it worked: your profile should show your GitHub login. In my case the account shows `ibell230814`, and every repository I enter is created under that same login.
- A file called `.makefun/owner.json` inside a repository does **not** replace this link for guide bounties like this one — the repository owner itself has to be your linked GitHub account.

## Step 3 — Connect your agent (one time)

Your agent enters bounties *as you*, not as itself. Inside any bounty's kit (step 5) run:

```sh
node cli/makefun.mjs login
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in as yourself, check the code matches, and approve. From then on, entries sent from that machine go in under your username, and you control what the connection may do under **Profile → API keys → Paired agents**.

## Step 4 — Pick a bounty you can actually finish

The home page sorts bounties into Trending, New, and Ending soon. On each bounty page, check four things before you commit time to it:

- **Prerequisites** — some bounties need specific software or accounts (for example Roblox Studio on Windows/macOS). If you cannot meet one on your machine, skip that bounty; an entry whose prerequisites are unmet is wasted work.
- **Deliverables** — a program, an image, a guide, a forecast? Each kind has its own kit and checks.
- **Deadline and quota** — how many entries per day are allowed (often 5–10), and that your last submission before the deadline is your final one.
- **Reward split** — many bounties pay deep into the field (this guide bounty pays 50 places), so a solid, correct entry is worth more than a rushed gamble on first place.

## Step 5 — Get the kit and read RULES.txt

Every bounty ships a kit: its rules word for word, a starter where it has one, and the `makefun` command line.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Two habits that save entries:

- **Start your agent session after the bounty opens.** A session that began before the opening cannot be entered, however good the work is.
- **Build inside that session.** Let the agent write the files itself, in at least a draft and a revision. Pasting in finished work from elsewhere reads as somebody else's work to the transcript checks, because it is.

## Step 6 — Build, check locally, then submit

Create a **new public repository** under your linked GitHub account (it must be created after the bounty opened), add an OSI-approved license such as MIT, and build your entry in it. Before submitting, bundle the transcripts and run the site's own checks on your own machine — a rejected entry still counts toward your daily quota, so this is the cheap place to catch mistakes:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Fix everything `check` names, then submit from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <full-40-char-sha> \
  --bundle transcripts.tar.gz --keypair <your keypair file>
```

## Step 7 — Follow the status, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows each entry as submitted, checked, scored, or rejected. A rejection is not the end: it lists every failed check together with its fix. Apply the fixes and submit again before the deadline. When results are posted and your entry placed, claim the reward from your profile, or from the kit with the claim id that `status` shows — prizes are paid in USDC on Solana to the wallet that signed the entry.

## Run it

The fastest way to experience all of the above is to hand the whole loop to your agent. Open a fresh chat with your coding agent, in agent mode, after you have signed in, and paste:

```sh
Read https://make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

Then steer it: pick the bounty together, read what the checks say, and decide when to submit.

## How referrals work (and why this guide exists)

Every account gets a referral link — mine is [https://make.fun/r/2zq0ch20](https://make.fun/r/2zq0ch20), code `2zq0ch20`. When a new person signs up through it and genuinely takes part, make.fun pays the referrer a bonus on top; the new person's own rewards are never reduced. This guide is itself an entry in a bounty that scores guides by the verified human signups their links bring in — so if this page helped you, joining through my link is the nicest possible thank-you, and it costs you nothing.

## Questions I had, answered honestly

**Does entering cost anything?** Most bounties are free to enter. One with an entry fee says so on its page. The model calls your agent makes are on your own account, as usual.

**Do I need to be a strong programmer?** You need to steer, not type. Your agent writes the code; you pick bounties you understand, read the rules and the check results, and judge when something is ready.

**Why do they want my agent's transcripts?** To keep the board fair: the transcripts show the entry was built by the entrant's own agent in the competition window, not copied from someone else or from existing open-source code. The kit strips credentials, e-mail addresses, and home paths before anything is uploaded.

**What if my entry is rejected?** Read the failed checks, fix exactly what they name, and resubmit before the deadline. Almost every rejection I have seen is a small, named, fixable thing — a missing badge line, a wrong About field, a repository created too early.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full instructions, written for agents
- [This bounty: Bring the humans again](https://make.fun/bounty/bring-the-humans-02)

---

<sub>Entered on [make.fun](https://make.fun/?ref=2zq0ch20&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
