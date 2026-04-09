# PawPal Hardening Case Study

## Introduction
This is a recruiter-facing case study derived from recent biweekly write-ups.

## Key Themes

- **Boundaries:** Ensure proper boundaries are established in session management, preventing unauthorized access.
- **Validation:** Robust input validation mechanisms must be implemented to reject any malformed or unexpected inputs.
- **Session State Treated as Untrusted:** Any session state data must be treated as potentially untrusted, active defense mechanisms should be deployed.
- **Persistence Hardening:** Data persisted in the system should be properly secured against unauthorized access.
- **Tests as Verification:** Rigorously verifying functionality through tests remains essential for maintaining integrity.
- **Framework Mapping:** Understanding how the application framework handles security is critical.

## Useful Links
- [PawPal Repository](https://github.com/JasmineSutton/PawPal)
- [SECURITY_README.md](https://github.com/JasmineSutton/PawPal/blob/af862c49cb5f1e4ecc3b77f43fbd64890e68c3be/SECURITY_README.md)