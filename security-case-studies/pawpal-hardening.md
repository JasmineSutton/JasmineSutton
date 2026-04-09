# PawPal Hardening Case Study

## Introduction
PawPal+ is a Streamlit-based pet care scheduling application built around four core classes: `Owner`, `Pet`, `Task`, and `Scheduler`. The project needed to handle task creation, recurrence, conflict detection, and daily schedule generation, but I also used it to tighten validation, trust boundaries, and local persistence so the scheduler would behave predictably under bad or edge-case input.

My focus was on enforcing backend validation, maintaining clear boundaries between the UI and the data model, hardening local JSON persistence, and verifying that the scheduling logic behaved predictably under invalid or edge-case input.

## Threats / Abuse Cases Considered
The main risks considered were:

- malformed or unexpected input reaching scheduling logic
- UI-driven state changes bypassing backend validation
- unbounded in-memory growth from excessive pets or tasks
- corrupted JSON persistence causing silent data loss or inconsistent state

## Key Themes

- **Boundaries:** Validation stays in the backend model layer rather than being trusted to the Streamlit UI.
- **Validation:** Names, times, species, frequencies, priorities, and durations are validated before use.
- **Session State Treated as Untrusted:** UI-driven state changes are routed through validated model methods instead of directly mutating attributes.
- **Persistence Hardening:** Local JSON persistence uses atomic writes and explicit corrupted-file handling.
- **Tests as Verification:** Scheduling, recurrence, validation reuse, and persistence behavior are verified through automated tests.
- **Framework Mapping:** The control set aligns most closely with OWASP A03/A04 and NIST SI-10 style validation and design-hardening concepts.

## Key Decisions

### Backend validation as the trust boundary
Validation is enforced in `pawpal_system.py` during object construction and update paths. That includes sanitization, strict `HH:MM` parsing, and allowlist checks for values such as frequency and species. This prevents invalid input from reaching scheduling, persistence, or display logic through alternate entry paths like tests or JSON reloads.

**Evidence:** `pawpal_system.py` (`_sanitize`, `_validate_time`, `Task.__post_init__`, `Pet.__post_init__`, `Owner.__init__`)

### UI state was not treated as authoritative
One issue identified during review was that owner name changes in the UI were bypassing the original validation path. I corrected that by adding `Owner.set_name()` and routing UI-driven updates through the same backend validation used at initialization.

**Evidence:** `pawpal_system.py` (`Owner.set_name`); `app.py` (owner update path calls `set_name`)

### Persistence was hardened after JSON support was added
Once local persistence was introduced, JSON writes were made atomic and corrupted JSON loads were handled explicitly. Rather than silently treating a bad save file as normal empty state, the system records a warning condition so the UI can surface that to the user.

**Evidence:** `pawpal_system.py` (`Owner.save_to_json`, `Owner.load_from_json`, `Owner.last_load_warning`); `app.py` (warning surfaced to user)

### Resource usage was bounded
The project enforces explicit limits on name length, description length, pets per owner, and tasks per pet. This keeps scheduling operations predictable and avoids unbounded in-memory growth even in a local demo setting.

**Evidence:** `pawpal_system.py` (`MAX_NAME_LENGTH`, `MAX_DESCRIPTION_LENGTH`, `MAX_TASKS_PER_PET`, `MAX_PETS_PER_OWNER`)

## Verification
Automated tests cover:

- task addition and completion
- chronological and priority-first sorting
- daily and weekly recurrence, including preserving duration and priority
- duplicate pet name rejection
- invalid time rejection
- exact-match conflict detection, including conflict and no-conflict scenarios
- JSON save/load behavior, missing-file handling, and corrupted JSON warning behavior
- owner rename validation reuse

Current status: **19 tests passing**

**Evidence:** `tests/test_pawpal.py`

## Reproduction

```bash
python -m pytest -q
```

## Framework Mapping

- **OWASP A03:2021 (Injection)**
  - sanitization and allowlist validation
  - strict time parsing

- **OWASP A04:2021 (Insecure Design)**
  - bounded collections
  - safer persistence behavior
  - controlled update paths

- **NIST SP 800-53 SI-10 (Information Input Validation)**
  - validation of names, times, species, priorities, frequencies, and durations

## Useful Links
- [PawPal Repository](https://github.com/JasmineSutton/PawPal)
- [SECURITY_README.md](https://github.com/JasmineSutton/PawPal/blob/af862c49cb5f1e4ecc3b77f43fbd64890e68c3be/SECURITY_README.md)
