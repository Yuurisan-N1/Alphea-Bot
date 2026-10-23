<div align="center">

<img width="100%" alt="header" src="https://capsule-render.vercel.app/api?type=waving&height=210&text=Alphea%20Connect%20Bot&fontAlign=50&fontAlignY=36&fontSize=56&desc=Email%20Code%20Sign%20In%20%7C%20Session%20Renewal%20%7C%20Quest%20Claim%20%7C%20Point%20Balance&descAlign=50&descAlignY=58"/>

<img alt="typing" src="https://readme-typing-svg.demolab.com?font=Inter&size=18&duration=3000&pause=650&center=true&vCenter=true&width=900&lines=Email+Code+Sign+In+%7C+Typed+Once+Only;Session+Renewed+Automatically+%7C+No+Second+Code;Every+Ready+Quest+Claimed+In+One+Pass;Live+Point+Balance+%7C+Today+And+All+Time;Built+by+Yuurisandesu"/>

<p>
  <img alt="python" src="https://img.shields.io/badge/Python-3.12+-3776AB?logo=python&logoColor=white"/>
  <img alt="platform" src="https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20Termux-111111"/>
  <img alt="accounts" src="https://img.shields.io/badge/Accounts-Single-111111"/>
  <img alt="proxy" src="https://img.shields.io/badge/Proxy-Supported-111111"/>
  <img alt="author" src="https://img.shields.io/badge/by-Yuurisandesu-111111"/>
</p>

<p>
  <b>Alphea Connect</b> is a terminal bot for the Alphea Connect app.<br/>
  It signs in with the email verification code, keeps that session alive on its own, claims every quest the app has ready, and prints the point balance after each pass.<br/>
  The code is typed once. From then on the bot renews itself and never asks for another one until the session is truly refused.<br/>
  Built and distributed by <b>Yuurisandesu</b>.
</p>

</div>

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Bot](#running-the-bot)
- [Features](#features)
- [File Structure](#file-structure)
- [Disclaimer](#disclaimer)

---

## Requirements

- Python `3.12+`
- Git
- An Alphea Connect account, the email address on it is the only credential the bot needs

---

## Installation

### Install Python

**Windows:**

1. Download the installer from https://www.python.org/downloads/ and run it
2. On the first screen tick **Add python.exe to PATH**, then continue with the default options
3. Open Command Prompt and verify the installation:

```cmd
python --version
```

**Linux (Ubuntu / Debian):**

```bash
sudo apt-get update
sudo apt-get install -y python3 python3-pip
python3 --version
```

**macOS:**

```bash
brew install python
python3 --version
```

> If you do not have Homebrew, install it first from https://brew.sh

**Termux (Android):**

```bash
pkg update && pkg upgrade -y
pkg install python -y
python --version
```

---

### Clone and Install

**Clone the repository:**

```bash
git clone https://github.com/Yuurisan-N1/Alphea-Bot.git
cd Alphea-Bot
```

**Install dependencies:**

```bash
pip install -r requirements.txt
```

---

## Configuration

### 1. Email (data.txt)

One email address, the one registered on the app:

```
your.address@gmail.com
```

Blank lines and lines starting with `#` are ignored. The bot runs a single account, so only the first
line is read.

### 2. Proxy (proxy.txt)

Fill `proxy.txt` with one proxy on the first line (optional, leave the file empty to run direct):

```
host:port
host:port:user:pass
http://user:pass@host:port
```

Only the first line is used, and the credentials are masked in the log output.

### 3. Bot Settings (config.json)

```json
{
  "settings": {
    "sleep_seconds": 3600
  }
}
```

`sleep_seconds` is the pause between passes, clamped to the range 120 to 86400. Nothing else belongs
in this file, and the bot rewrites it with these defaults if it is missing or unreadable.

---

## Running the Bot

```bash
python3 bot.py
```

---

## Features

### Email Code Sign In

The bot asks the app to send a code to the address in `data.txt`, prints that address in masked form,
and waits for you to type the code. Once the code is accepted the account profile is reported and the
session is stored, so the login is a one time step.

### Session Renewal

An access session only lives for as long as the app grants it. Before every pass the bot checks the
stored expiry, and when it has run out it renews the session by itself and saves the new one. The code
is only asked for again when the stored session is genuinely refused, which in a normal run never
happens.

### Quest Scan

Every pass starts by reading the quest list from the app and splitting it in two groups: the quests
that are ready to claim, and the quests that are still in progress. In progress quests are only
reported with their current amount against the target, so progress stays visible without touching
them.

### Quest Claim

Each ready quest is claimed in its own request, and every request carries a fresh identifier so the
same quest can never be credited twice even if the request is repeated. The reward is printed per
quest as it lands, and the pass closes with the total it earned. A quest the app refuses prints the
reason on one line and the pass carries on with the next one.

### Foreground And Referral Quests

Quests that expect the app to stay open on screen, and quests that expect real invited accounts, are
only reported. They are never forced, because the app reviews session duration and invitation
patterns, and a bot that fakes them is the exact pattern that gets an account flagged.

### Point Balance

After the claims the bot prints the current balance, the amount earned today and the all time amount,
so the effect of a pass is visible straight away.

### Cooldowns

There are no invented delays. The only waits are the `sleep_seconds` from `config.json` and a wait the
app itself asks for, which is honoured once and then retried. There is no backoff ladder, no margin on
the expiry check and no pause between claims.

### Proxy Support

The first line of `proxy.txt` is applied to the whole session. Credentials are never printed, the log
only shows a masked form, and an empty file runs the bot direct.

---

## File Structure

```text
Alphea-Bot/
├── bot.py            # Sign in, session renewal, quest claim and balance report
├── config.json       # sleep_seconds only
├── data.txt          # Email address, one line
├── proxy.txt         # Proxy list, first line used (optional)
├── requirements.txt  # Python dependencies
├── session.json      # Created at runtime, holds the saved session
├── LICENSE           # AGPL-3.0
├── README.md         # This file
└── utils/
    ├── __init__.py
    └── banner.py     # Banner using yuurisan module
```

---

## Disclaimer

This tool is built for educational and technical exploration purposes. Automating the app may violate
its terms of service and can lead to an account ban. Use it wisely and at your own responsibility, and
only on the account you own.

---

<div align="center">
<img width="100%" alt="footer" src="https://capsule-render.vercel.app/api?type=waving&height=120&section=footer"/>
</div>
