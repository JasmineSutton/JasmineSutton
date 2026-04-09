# Playlist Chaos Hardening Case Study

## Introduction
This case study explores the security hardening measures implemented in the Playlist Chaos project to safeguard against potential threats and vulnerabilities.

## Threats/Abuse Cases Considered
- Unauthorized access to user data.
- Manipulation of playlist data.

## Key Themes
- Centralized validation constraints.
- Session state treated as untrusted.
- Search hardening preserving wildcard usage.
- Resource bounds to prevent abuse.

## Key Decisions
- Implemented centralized validation constraints to validate input.
- Treated session state as untrusted to prevent session hijacking.
- Employed search hardening techniques to ensure valid results, even with wildcards.
- Defined resource bounds to limit user actions and prevent abuse.

## Verification
To verify the hardening measures implemented:
1. Run unit tests using:
   ```bash
   python -m unittest -v test_security.py
   ```
2. Execute Streamlit application:
   ```bash
   streamlit run app.py
   ```

## Framework Mapping
- High-level mapping of security measures to industry best practices.

## Useful Links
- [Playlist Chaos Repository](https://github.com/JasmineSutton/PlaylistChaos)
- [Playlist Chaos Security.md](https://github.com/JasmineSutton/PlaylistChaos/blob/main/Security.md)
