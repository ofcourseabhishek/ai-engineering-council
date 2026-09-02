# Browser Agent Protocol

Use this reference when coordinating external web-chat reviewers.

## Preferred reviewers

Use available services such as:

- Claude web
- Grok web
- Gemini web

The exact provider set is opportunistic. Never fabricate participation.

## Authentication

- Prefer an existing authenticated browser session.
- If login is required, ask the user to complete authentication themselves.
- Never type or store passwords, OTPs, recovery codes, API keys, cookies, or session tokens.
- Never bypass CAPTCHA or anti-bot mechanisms.

## Independent-round discipline

For proposal round:

1. Use a new chat/thread when feasible.
2. Send the same frozen specification.
3. Do not mention what other models proposed.
4. Ask for a self-contained answer matching the proposal template.

For critique round:

1. Replace provider identities with Proposal A/B/C/D.
2. Remove obvious self-identifying language if practical.
3. Ask the reviewer to rank *arguments*, not models.

## Reliability checks

After every browser send:

- confirm the message actually sent,
- wait for a complete answer,
- verify the active tab/provider,
- check for rate-limit or error banners,
- avoid copying partial streaming text,
- save the answer before moving to another round.

## Data minimization

Never paste the whole repository by default. Share:

1. frozen specification,
2. relevant interfaces,
3. small sanitized snippets,
4. test expectations.

Do not send secrets or unrelated proprietary code.

## Provider failure

If a provider cannot be used because of authentication, UI breakage, a rate limit, CAPTCHA, account restrictions, or automation blocking:

1. record the provider as unavailable,
2. do not try to bypass the restriction,
3. continue with remaining reviewers when the selected council mode permits,
4. do not claim the unavailable provider participated.
