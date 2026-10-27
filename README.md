# Voice Interview Coach

Practice job interviews out loud with a real-time voice coach. Choose a role and
experience level; the coach asks a short sequence of role-specific questions,
offers practical feedback on each answer, and ends with a concise recap.

This is an application built on the LiveKit Agents framework. The framework and
provider integrations in this repository remain LiveKit's work; see
[LICENSE](LICENSE), [NOTICE](NOTICE), and the [upstream project](https://github.com/livekit/agents)
for attribution and licensing.

## What it does

- Interview tracks for backend engineering, data analysis, and product management.
- Beginner, intermediate, and advanced question plans.
- Four questions per session, asked one at a time.
- Feedback based on what you actually say, without hiring predictions or objective
  candidate scores.
- Local unit tests for the question plans and settings; no API key is needed for
  those tests.

## Quick start

Requirements: Python 3.10 or later, an OpenAI API key, and a microphone/speaker
for voice practice. LiveKit server credentials are additionally needed for
`dev` mode, but not for local console mode.

From the repository root in PowerShell:

```powershell
Set-Location .\projects\voice-interview-coach
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -e .
Copy-Item .env.example .env
```

Add your `OPENAI_API_KEY` to `.env`, then start a local session:

```powershell
voice-interview-coach console
```

To connect through a LiveKit project, set `LIVEKIT_URL`, `LIVEKIT_API_KEY`, and
`LIVEKIT_API_SECRET` in `.env`, then run:

```powershell
voice-interview-coach dev
```

Console mode avoids a LiveKit server, but still uses the configured OpenAI service.
Model requests require network access and are subject to the provider's data
handling terms.

## Configure the interview

Set these optional values in `projects/voice-interview-coach/.env`:

| Variable | Default | Available values / purpose |
|---|---|---|
| `INTERVIEW_ROLE` | `backend engineer` | `backend engineer`, `data analyst`, `product manager` |
| `INTERVIEW_LEVEL` | `intermediate` | `beginner`, `intermediate`, `advanced` |
| `OPENAI_LLM_MODEL` | `gpt-4o-mini` | OpenAI model used for responses |
| `OPENAI_TTS_VOICE` | `echo` | OpenAI text-to-speech voice |

Unsupported role or level values are rejected with an actionable error.

## Run tests

From `projects/voice-interview-coach`, with its virtual environment active:

```powershell
python -m unittest discover -s tests -v
```

These tests exercise local question selection and configuration only; they do not
make API calls.

## Project layout

```text
projects/voice-interview-coach/
  src/interview_coach/
    agent.py       Interview behavior and coaching instructions
    questions.py   Curated questions by role and level
    settings.py    Environment configuration and validation
    worker.py      LiveKit worker and voice-session setup
  tests/           Offline unit tests
  README.md        App-specific setup and usage
```

The LiveKit Agents framework and provider plugins are retained in the repository's
`livekit-agents/` and `livekit-plugins/` directories. They provide the worker,
voice-session, and provider integration layers used by the app.

## Limitations

- The question sets are curated examples, not a validated interview curriculum.
- Feedback quality depends on the configured model and the clarity of the answer.
- The app does not persist transcripts, provide analytics, or objectively score
  candidates.
- A live interview requires access to the configured OpenAI service.

For framework documentation and broader examples, see the
[LiveKit Agents documentation](https://docs.livekit.io/agents/) and
[upstream repository](https://github.com/livekit/agents).
