# Ayjan Minutes

**Record a meeting. Get the minutes.** A Windows desktop app that captures your mic and
your system audio together, transcribes the recording **on your own machine**, and turns it
into a formatted Word document with agenda items, decisions and colour-coded action items.

Your audio never leaves your computer. Only the finished transcript text is sent for
summarisation — and if you bring your own API key, you choose who sees it.

[**Download**](https://ayj-systems.github.io/ayjanminutes/) ·
[About](https://ayj-systems.github.io/ayjanminutes/about.html) ·
[Release notes](https://github.com/AYJ-Systems/ayjanminutes/releases)

<img width="736" height="645" alt="Ayjan Minutes main window" src="https://github.com/user-attachments/assets/a1fa4830-26e3-4ffd-9131-0ff57e62fcd8" />

---

## Install

1. Download `Ayjan-Minutes-Setup.exe` from the [website](https://ayj-systems.github.io/ayjanminutes/).
2. Run it. The installer is ~15 MB and pulls the app down during setup — no Python, no
   command line.
3. Tick **Download transcription model** if you want the app ready to go before your first
   meeting. Skip it and the model downloads on your first transcription instead.

Windows 10 or 11. Updates arrive in-app: a pill button appears when a new version is out,
and one click installs it.

## First run

Three ways in, on the sign-in screen:

| | What you get |
|---|---|
| **Try free — no account** | 5 recordings. No email, no password, no card. |
| **Sign up** | Free account, 5 AI minutes per month, keeps your usage across reinstalls. |
| **Sign in with Google** | Same as above, one click. |

Forgot your password? The **Forgot password?** link emails a reset and walks you through
the new one without leaving the app.

> Guest usage doesn't transfer. If you start as a guest and create an account later, the
> account begins with a fresh set of free recordings.

## How it works

**1 — Record.** Pick your microphone and your speaker, hit record. The speaker selection is
what captures the *other* people: Teams, Zoom, Meet, a browser call, anything playing
through Windows. The transcript preview fills in live as you talk, so there's little to wait
for when you stop.

**2 — Transcribe.** `faster-whisper` runs locally. No internet needed for this step. On an
NVIDIA card it's 3–5× faster; on CPU it still works, just slower.

**3 — Minutes.** The transcript goes to an AI summariser and comes back as a Word document.

Everything lands in your Documents folder:

```
Documents/Ayjan Minutes/
├── recordings/        meeting_20260725_143022.wav     stereo, 48 kHz
├── transcripts/raw/   meeting_20260725_143022.txt     plain text
└── meeting_minutes/   meeting_20260725_143022_minutes.docx
                       meeting_20260725_143022_minutes.txt
```

## What the minutes look like

The Word document is built from a template and includes:

- **Header** — project, date, time, location, attendees, apologies, objective
- **Agenda items** — a table per item: issues raised, decisions made, actions agreed
- **Open questions** — anything left unresolved
- **Next meeting** — date and time, if it came up
- **Action items summary** — colour-coded by owner: 🟢 internal, 🟡 external, 🔵 cross-party

**Getting names right.** Whisper mangles unusual names. Add the people you meet with to the
**Names** tab and the AI corrects the spelling for you — "Ning" becomes "Naing". They're
treated as *possible* attendees, so listing someone who wasn't there does no harm; only
names that actually appear in the transcript are used.

## Plans

| | Free | Pro |
|---|---|---|
| Recording | Unlimited | Unlimited |
| Local transcription | Unlimited | Unlimited |
| AI meeting minutes | 5 per month | Unlimited |
| Your own Claude / OpenAI key | ✓ | ✓ |

Pro comes as a monthly or an annual plan — current pricing is on the
[website](https://ayj-systems.github.io/ayjanminutes/).

Upgrade under **Settings → Account → Upgrade to Pro**. Payment goes through Paddle's
checkout; your plan switches over by itself once it clears, usually within seconds. Pro is
tied to your account, so it follows you to any machine you sign in on.

To cancel or get an invoice, use the link in your Paddle receipt email. Refunds: see the
[refund policy](https://ayj-systems.github.io/ayjanminutes/refunds.html). Note that a
refund alone doesn't end a subscription — cancel it as well.

## AI providers

**Settings → AI Provider** — three options:

| Provider | Key needed | |
|---|---|---|
| **Built-in AI** | No | The default. Works the moment you sign in. Counts against your plan. |
| **Claude** | Yes | Your key from [console.anthropic.com](https://console.anthropic.com). Billed by Anthropic, unlimited from our side. |
| **OpenAI** | Yes | Your key from [platform.openai.com](https://platform.openai.com). Same deal. |

Keys are stored locally and reused next session. Built-in AI is the one to use unless you
have a reason not to.

## Transcription models

Bigger is more accurate and slower. **medium** is the default and a good balance. Change it
under **Settings → Model**.

| Model | Download | Relative speed | Accuracy |
|---|---|---|---|
| tiny | 39 MB | fastest | lower |
| base | 140 MB | very fast | good |
| small | 244 MB | ~1× | better |
| medium | 769 MB | ~2× slower | very good |
| turbo | ~809 MB | fast | very good |
| large-v3 | ~1.5 GB | ~4× slower | best |

Models cache to `%USERPROFILE%\.cache\ayjan_minutes\models\` — downloaded once, then reused.
Safe to delete that folder to reclaim space; it re-downloads when next needed.

### Using your GPU

Transcription runs on CPU unless it finds something better. For an NVIDIA card:

1. Install [CUDA Toolkit 12.x](https://developer.nvidia.com/cuda-toolkit-archive) —
   Windows, x86_64, `exe (local)`, default settings.
2. Restart the app. The device indicator in the Transcription panel reads **CUDA** when it's
   working.

**Settings → Device** controls this: **Auto** (GPU if present, CPU otherwise — the default),
**CUDA** (GPU only), **CPU** (never use the GPU).

CPU is fine for short recordings and the small models. Use the GPU for medium/large or
anything over half an hour.

## Troubleshooting

**No audio in the recording**
Check the **Select Microphone** and **Select Speaker** dropdowns, and confirm both devices
work in Windows Sound settings. If only *your* voice is missing after a Windows or driver
update, the app already works around a known WASAPI bug — update to the latest version.

**First transcription takes forever**
It's downloading the model — one time only. Later runs use the cache. Switch to tiny or base
under **Settings → Model** if you want speed over accuracy.

**No minutes generated**
Confirm **Settings → Enable Summarization** is on. On Built-in AI, check you're signed in.
On Claude/OpenAI, check the key under **Settings → AI Provider**. If you're on Free and out
of monthly minutes, it'll tell you.

**Can't sign in**
Sign-in needs a network connection. Use **Forgot password?** to reset. With no connection
the app opens in offline mode — recording and transcription still work, minutes don't.

**GPU errors after installing CUDA**
Reboot. If it persists the app falls back to CPU on its own, so nothing is lost.

**Paid but still showing Free**
The "Waiting for Payment" window has an **I've paid — refresh** button. Failing that,
restart the app. If it's still Free after a few minutes, get in touch.

## Privacy

- **Audio never leaves your machine.** Recording and transcription are entirely local.
- **Transcript text** is sent to the AI provider you selected, to produce the minutes.
- **No payment details** ever reach us — Paddle handles that, and is the merchant of record.
- Full detail in the [privacy policy](https://ayj-systems.github.io/ayjanminutes/privacy.html).

## Limitations

- Windows 10/11 only — system-audio capture relies on WASAPI loopback.
- English audio only. Non-English recordings are detected and skipped with a warning.
- Accuracy tracks audio quality: background noise, low volume and people talking over each
  other all cost you.

---

[Terms](https://ayj-systems.github.io/ayjanminutes/terms.html) ·
[Privacy](https://ayj-systems.github.io/ayjanminutes/privacy.html) ·
[Refunds](https://ayj-systems.github.io/ayjanminutes/refunds.html)
