# Q&A: WebSocket Message Integrity (HMAC) for Chat Messages

## Question
Apart from TLS, do we also impose message integrity checks (e.g., HMAC) for the chat messages received via WebSocket from end users, to ensure that in case of partner app compromise (e.g., XSS) it is still difficult to affect our system?

## Tech Lead's Answer
Yes, in addition to TLS encryption, each chat message sent via WebSocket is signed with an HMAC using a key unique to the partner and session. The Sprinklr backend verifies the HMAC on every message before processing. This ensures that even if a partner app is compromised (e.g., via XSS), it is difficult for an attacker to forge valid messages without access to the HMAC key.

## Recommendation
Continue enforcing HMAC-based message integrity for all WebSocket chat messages. Rotate HMAC keys per session and partner, and never expose them to the browser.

## Risk
If HMAC keys are leaked or not rotated, attackers could forge messages, potentially leading to abuse or system compromise.

## Mitigation
- Store HMAC keys securely on the backend only.
- Rotate keys per session and partner.
- Monitor for failed HMAC validations and alert on anomalies.

## Incident Response Plan
- Invalidate affected sessions and keys.
- Investigate the source of key leakage or forgery attempts.
- Notify affected partners and update security controls.
