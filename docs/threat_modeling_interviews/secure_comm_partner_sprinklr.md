# Q&A: Secure Communication Between Partner and Sprinklr Service

## Question
How are we ensuring secure communication between partner and Sprinklr service?

## Tech Lead's Answer
All communication between partner systems and Sprinklr services is secured using mutual TLS (mTLS) for both API and WebSocket connections. Only trusted certificates are accepted, and connections are restricted to whitelisted IPs where possible.

## Recommendation
Continue enforcing mTLS for all partner-to-Sprinklr communication. Regularly audit certificate trust stores and IP whitelists.

## Risk
If mTLS is misconfigured or certificates are compromised, attackers could intercept or impersonate traffic.

## Mitigation
- Automate certificate rotation and revocation.
- Monitor for failed or suspicious connection attempts.
- Enforce strict IP whitelisting.

## Incident Response Plan
- Revoke compromised certificates.
- Update trust stores and IP whitelists.
- Notify affected partners and rotate credentials.
