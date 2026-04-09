# PawPal Hardening Case Study

## Introduction
PawPal+ is a Streamlit-based pet care scheduling application built around four core classes: `Owner`, `Pet`, `Task`, and `Scheduler`. While the functional goal was to support task creation, recurrence, conflict detection, and daily schedule generation, I treated the project as a security and reliability exercise as well. My focus was on enforcing backend validation, maintaining clear trust boundaries between the UI and the data model, hardening local JSON persistence, and verifying that the scheduling logic behaved predictably under invalid or edge-case input.

## Key Themes

- **Boundaries:** Validation stays in the backend model layer rather than being trusted to the Streamlit UI.
- **Validation:** Names, times, species, frequencies, priorities, and durations are validated before use.
- **Session State Treated as Untrusted:** UI-driven state changes were routed back through validated model methods instead of directly mutating attributes.
- **Persistence Hardening:** Local JSON persistence was hardened with atomic writes and corrupted-file handling.
- **Tests as Verification:** Scheduling, recurrence, validation reuse, and persistence behavior were verified through automated tests.
- **Framework Mapping:** The control set aligns most closely with OWASP A03/A04 and NIST SI-10 style validation and design-hardening concepts.

## Key Decisions

### Backend validation as the trust boundary
Validation is enforced in `pawpal_system.py` during object construction and update paths. That includes sanitization, strict `HH:MM` parsing, and allowlist checks for values such as frequency and species. This prevents invalid input from reaching scheduling, persistence, or display logic through alternate entry paths like tests or JSON reloads.

### UI state was not treated as authoritative
One issue identified during review was that owner name changes in the UI were bypassing the original validation path. I corrected that by adding `Owner.set_name()` and routing later updates through the same backend validation logic used at initialization.

### Persistence was hardened after JSON support was added
Once local persistence was introduced, I added atomic file-write behavior and explicit handling for corrupted JSON. Rather than silently treating a bad save file as normal empty state, the system records a warning condition so the UI can surface that to the user.

### Resource usage was bounded
The project enforces explicit limits on name length, description length, pets per owner, and tasks per pet. This keeps scheduling operations predictable and avoids unbounded in-memory growth even in a local demo setting.

## Verification

Automated tests covered:

- task completion
- task addition
- chronological sorting
- priority-first sorting
- daily and weekly recurrence
- recurrence preserving duration and priority
- duplicate pet name rejection
- invalid time rejection
- exact-match conflict detection
- JSON save/load behavior
- missing-file handling
- corrupted JSON warning behavior
- owner rename validation reuse

Current status: **19 tests passing**

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
