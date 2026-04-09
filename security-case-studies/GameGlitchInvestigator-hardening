# Game Glitch Investigator Hardening Case Study

## Introduction
Game Glitch Investigator is a Streamlit-based debugging project built around a simple game flow, but the codebase had structural and security issues that went beyond basic bug fixing. The main problems were that the UI and game logic were too tightly coupled, session-driven behavior was easy to abuse, and there was no real accountability for security-relevant actions inside a session.

My focus was on availability, state integrity, and session-level accountability. I separated core logic more cleanly, added rate limiting to reduce rapid session abuse, hardened the persisted high-score handling, and introduced a session-scoped audit trail so important actions were recorded with timestamps.

## Threats / Abuse Cases Considered
The main risks considered were:

- rapid repeated submits causing excessive reruns or unstable session behavior
- sustained submit spam over time within the same session
- malformed or tampered local high-score data affecting app state
- missing auditability for submits, wins, losses, and reset events
- Streamlit reruns resetting control state if security state was not persisted in session memory

## Key Themes

- **Boundaries:** Security controls had to survive Streamlit reruns, so limiter and audit state were kept in session state instead of local variables.
- **Validation:** Persisted high-score data is normalized before use instead of being trusted blindly.
- **Session State Treated as Untrusted:** Session-driven actions were guarded with explicit rate controls and logged when blocked.
- **Availability Hardening:** Cooldown and rolling-window submit limits were added to reduce session abuse and unnecessary reruns.
- **Auditability:** A session-scoped event log was added to record security-relevant actions with timestamps.
- **Tests as Verification:** Gameplay logic is tested automatically, while the added security controls are currently verified primarily through manual testing.
- **Framework Mapping:** The control set lines up most directly with STRIDE Denial of Service and Repudiation, along with OWASP A04/A09 and NIST SC-5, AU-2, AU-12, and SI-10 style controls.

## Key Decisions

### Defensive handling for persisted high-score data
The app now reads and writes the high score through a fixed local file path, converts stored values to integers, clamps negatives to `0`, and falls back safely if the file is missing or malformed. This does not make the file tamper-resistant, but it does prevent bad local data from crashing the app or pushing invalid score values into state.

**Evidence:** `app.py` (`HIGH_SCORE_FILE`, `load_high_score`, `save_high_score`)

### Rate limiting was added at the submit boundary
I added two submit controls: a short cooldown between clicks and a rolling-window cap for total submits. The cooldown handles burst clicking, while the rolling window handles sustained spam over time. Together they reduce unnecessary reruns and make session abuse harder.

**Evidence:** `app.py` (`Submit_cooldown_seconds`, `Max_Submits_per_Window`, `Rate_window_seconds`, cooldown check, rolling-window check)

### Security state was persisted in session memory
Because Streamlit reruns the script on interaction, limiter timestamps and audit data had to live in `st.session_state` or they would reset on the next click and become trivial to bypass. Persisting that state across reruns is what makes the controls enforceable.

**Evidence:** `app.py` (`st.session_state.last_submit_ts`, `st.session_state.submit_timestamps`, `st.session_state.event_log`)

### New-game reset behavior also resets control state
Starting a new game now clears the active limiter window and writes a `new_game` event to the audit trail. That keeps limiter state from leaking across game sessions and makes the event timeline easier to interpret during review.

**Evidence:** `app.py` (new-game branch resets limiter state and appends `new_game` event)

### Security-relevant actions are logged with timestamps
The app now records submit attempts, cooldown blocks, rolling-window rate-limit blocks, wins, losses, and new-game resets. The log is session-scoped rather than durable, but it still improves traceability and makes it easier to understand what happened when a session behaves unexpectedly.

**Evidence:** `app.py` (`event_log` initialization and append calls for `new_game`, `submit`, `rate_limited_cooldown`, `rate_limited_window`, `win`, `loss`)

## Verification
Current verification is split between automated testing and manual validation.

Automated tests currently cover gameplay logic and one state-initialization check.

Manual verification covers the hardened controls, including:

- malformed `high_score.json` falling back safely instead of crashing
- cooldown-based submit blocking
- rolling-window rate limiting
- event-log entries for `new_game`, `submit`, `rate_limited_cooldown`, `rate_limited_window`, `win`, and `loss`

Current gap: there are not yet dedicated regression tests for the rate limiter, audit log, or malformed high-score handling.

**Evidence:** `tests/test_game_logic.py`; `app.py`; `high_score.json` handling path

## Reproduction

```bash
python -m pytest
python -m streamlit run app.py
```

Manual checks:

1. Click `Submit Guess` rapidly to trigger the cooldown limiter.
2. Continue submitting until the rolling-window limit triggers.
3. Open `Developer Debug Info` and confirm the session event log records `new_game`, `submit`, rate-limit events, and end-state events.
4. Replace `high_score.json` with malformed JSON and confirm the app falls back safely instead of crashing.

## Residual Risks / Design Notes

- `Developer Debug Info` exposes the secret number and session event log. That is acceptable for the lab, but it would be an information-disclosure issue in a deployed version.
- The high-score file is normalized defensively, but it is still trusted local input and can be edited directly.
- The limiter is session-local because it relies on `st.session_state`. That is appropriate for a single-user lab app, but it is not the same as server-side rate limiting.
- The audit trail is session memory only. It improves accountability during a session, but it is not durable forensic logging.

## Framework Mapping

- **STRIDE - Denial of Service**
  - cooldown-based submit throttling
  - rolling-window rate limiting
  - session-persistent limiter state

- **STRIDE - Repudiation**
  - timestamped audit entries for submits, resets, wins, losses, and rate-limit blocks

- **OWASP A04:2021 (Insecure Design)**
  - explicit interaction limits
  - correct lifecycle handling for limiter state
  - defensive state management across reruns

- **OWASP A09:2021 (Security Logging and Monitoring Failures)**
  - session-scoped logging for security-relevant actions

- **NIST SP 800-53 SC-5 (Denial-of-Service Protection)**
  - cooldown and rolling-window controls

- **NIST SP 800-53 AU-2 / AU-12 (Event Logging / Audit Record Generation)**
  - audit trail for state-changing and security-relevant events

- **NIST SP 800-53 SI-10 / SI-11 (Input Validation / Error Handling)**
  - defensive normalization and safe fallback for persisted high-score data
