# Q&A: LLM Guardrails, Prompt Injection, Info Leakage, and Bias

## Question
How are we ensuring that LLM models (local or API-based) are not prone to prompt injection, information leakage, or bias?

## Tech Lead's Answer
All user and partner inputs to LLMs are passed through a guardrail layer that performs prompt sanitization, input validation, and context filtering. The guardrail layer detects and blocks known prompt injection patterns, strips sensitive data, and enforces content policies. LLM outputs are also post-processed to redact sensitive information and check for bias or policy violations.

## Recommendation
Continue to enhance the LLM guardrail layer with up-to-date detection patterns, context-aware filtering, and output moderation. Regularly audit LLM usage and responses for leakage and bias.

## Risk
If prompt injection or info leakage occurs, sensitive data could be exposed or LLMs could be manipulated to produce harmful outputs.

## Mitigation
- Maintain and update guardrail rules and detection patterns.
- Monitor LLM usage and outputs for anomalies.
- Use human-in-the-loop review for high-risk scenarios.

## Incident Response Plan
- Block affected LLM endpoints or sessions.
- Investigate and patch guardrail gaps.
- Notify stakeholders and update detection logic.
