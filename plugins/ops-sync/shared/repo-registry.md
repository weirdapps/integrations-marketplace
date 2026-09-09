# Repo Registry — Source of Truth

All repos live flat under `~/SourceCode/`. VPS mirror at same path via `ssh vps`. GitHub org: `weirdapps`.

## Repos

| Repo | Lang | Tests | Lint | CI | Branch | VPS |
|------|------|-------|------|----|--------|-----|
| atm-recon | py | — | — | — | master | no |
| claude-config | sh/py | pytest | shellcheck | gha | master | yes |
| etoro-portfolio | py | pytest | ruff | gha | master | no |
| etoro-tui | py | pytest | ruff | gha | master | no |
| etoro_census | ts | vitest | eslint | gha | master | yes |
| etoro_statement | py | pytest | ruff+mypy | gha | master | no |
| etoro_tickers | ts | — | — | gha | master | no |
| etorotrade | py | pytest | ruff+mypy | gha | master | yes |
| health | ts | vitest | eslint | gha | master | no |
| loans | py | pytest | ruff+mypy | gha | master | no |
| mockups | py | pytest | — | gha | master | no |
| news | py | pytest | ruff | gha | master | yes |
| outlook-access | ts | vitest | eslint | gha | master | no |
| plessas-lab | ts/py | vitest | ruff | gha | master | no |
| plessas-marketplace | py | — | ruff | gha | master | no |
| plessas-trading-stack | py | pytest | ruff | gha | master | no |
| remotion-private | ts | — | eslint | gha | master | no |
| remotion-studio | ts | vitest | eslint | gha | master | no |
| resume | ts | — | eslint | gha | master | no |
| sch-mail | py | — | — | — | master | no |
| second-brain | py | pytest | ruff | gha | master | yes |
| shared-workflows | yaml | — | — | gha | main | no |
| sw-utils | html | — | — | — | master | no |
| teams-access | ts | vitest | eslint | gha | master | no |
| telegram-bot | ts | vitest | eslint | gha | master | no |
| whatsapp-mcp | py/go | — | — | — | main | no |
| yahoo-access | py | pytest | ruff | gha | master | no |

Legend: gha = GitHub Actions, py = Python, ts = TypeScript, sh = Shell

**Repo visibility is deliberately not recorded here.** It is mutable and
safety-relevant (the PII-gauntlet rule depends on it), and a hand-maintained
copy of it drifted to nine wrong rows, every one understating public exposure.
Read it live instead: `gh repo list weirdapps --json name,visibility`.

> `communications-marketplace` was archived + deprecated on 2026-05-25 (superseded by `plessas-marketplace`) and removed from this registry on 2026-07-20.

## VPS Systemd Units

### Services (2 — continuous)

| Unit | Expected |
|------|----------|
| chat-watch.service | active/running |
| telegram-bridge.service | enabled (may be inactive) |

### Timers (~35 on VPS — core listed; extras noted below)

| Unit | Schedule (Athens) | Critical |
|------|-------------------|----------|
| config-sync | 06:00 daily | no |
| census-sync | 03:00 daily | no |
| news-digest | 00:00, 09:00, 13:00, 17:00, 21:00 | yes |
| news-monitor | bi-hourly 00:00–22:00 | yes |
| news-stack | 13:00 daily | no |
| sb-attachments | 02:00 daily | no |
| sb-auth-watch | 06:35, 12:00, 18:00 | yes |
| sb-calendar-sync | 06:33 daily | no |
| sb-curate-docs | 05:07 daily | no |
| sb-noon-catchup | 13:17 daily | no |
| sb-outlook-sync | hourly 07:00–22:00 | yes |
| sb-teams-sync | hourly 07:30–22:30 | yes |
| sb-daily-sync | 07:00 daily | yes |
| sb-reverse-ingest | 06:07 daily | no |
| sb-health-check | 23:50 daily | no |
| v3-report | 12:00 daily (renamed from committee) | yes |
| backtest | Sun 20:00 | no |
| gcloud-refresh | every 2h | yes |
| census-post | Sat 16:00 | no |
| daily-market-post | Mon–Fri 14:00 | no |
| monthly-review | 1st 20:00 | no |
| pi-pulse | Sun 22:00 | no |
| week-ahead | Sun 18:00 | no |
| daily-health | 23:55 daily | yes |

> Additional VPS timers active as of 2026-07-20 (not individually tabulated): vps-heartbeat, v3-ic-logger, news-market, brain-backup, signals-refresh, repo-autoupdate (05:43 FF-pull all repos), daily-summary, sb-conversation-sync, dependabot-sweep, logrotate-user, launchpadlib-cache-clean.

## Mac LaunchAgents (6)

| Label | Schedule | Critical |
|-------|----------|----------|
| com.automation.daily-health | 23:55 daily | yes |
| com.plessas.token-sync-vps | every 15 min | CRITICAL |
| com.trading.gcloud-auto-login | periodic | yes |
| com.user.brew-maintenance | Sun 10:00 | no |
| com.user.caffeinate-display | continuous | yes |
| com.weirdapps.viber-cleanup | 1st 03:30 | no |

## GitHub Actions Crons

| Repo | Workflow | Schedule (UTC) | Critical |
|------|----------|----------------|----------|
| etorotrade | daily-signals.yml | 22:00 daily | CRITICAL |
| etorotrade | ci.yml | push/PR | high |
| etoro_census | daily-census.yml | 00:00 daily | CRITICAL |
| etoro_census | deploy-pages.yml | triggered | high |

## VPS Connection

```
ssh vps   # alias in ~/.ssh/config
# Host: see the `vps` alias in ~/.ssh/config, User: <your-user>, Key: ed25519, ForwardAgent: yes
```
