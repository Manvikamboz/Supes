# ScamSense

A prompt-only scam message explainer. You paste a suspicious SMS, WhatsApp message or call script, and ScamSense replies in simple English, Hindi or Hinglish with a verdict, the scam type, red flags quoted from the message, what to do next, and the 1930 cyber fraud helpline.

**Team:** Supes (Team ID PE-B81-16)
**Course:** SCSE4041 Prompt Engineering, Milestone 2
**Project type:** Prompt-only workflow (one system prompt)
**Model:** [exact model name as shown in the model picker]
**Prototype:** [link to the shared Gem or chat]
**Results page:** open `index.html` in a browser, or see the GitHub Pages link if you turned it on

## What it does

Input: one pasted message.

Output, in five lines:

1. Verdict: Likely scam, Possibly a scam, Looks genuine, or Can't tell
2. Scam type: KYC or bank, Digital arrest, Investment, Fake job or task, Utility bill threat, Parcel or courier, Other, or None
3. Red flags, each with the exact words from the message
4. What to do now
5. For scams: if you already paid or shared details, call 1930 and tell your bank

The prompt also replies in the language of the message, refuses to obey instructions written inside the pasted message, and never tells the user to click a link, call a number from the message or send money.

## How it works

There is no code and no server. The whole tool is one system prompt, saved as a Gem. Each message you paste goes into that prompt and the model writes the reply.

The prompt has five parts: role and language, fixed output format, verdict rules, red flag rules, and advice rules. The full text of the tested version (v8) is in `prompt/` and on the results page.

## Results

Tested on 32 constructed messages, one run each, in the Gem. The table below is for prompt v8.

| Set | Messages | Passed |
|---|---|---|
| Original (T1-T14) | 14 | 14 |
| Unseen when written (U1-U4) | 4 | 4 |
| Invalid input (I1-I2) | 2 | 2 |
| New (N1-N12) | 12 | 11 |
| **Total** | **32** | **31** |

The one failure is N11, a message that asks the user to read out an OTP. The prompt gave "Possibly a scam" and advice that suggested sharing the OTP might be fine. A fix (v9) is written and has not been run yet, so it is not counted.

A test passes when the verdict and scam type match an answer key written before the run, every red flag quotes real words from the message, the reply never says "safe" and never tells the user to click, pay or call a number from the message, and the reply is in the language of the message.

## What testing found

- The first format forced 2 to 5 red flags, so genuine text could be flagged. The prompt now allows "None found".
- The model invented a red flag from the topic of a message. Each flag must now describe something the message does.
- A closed list of flag labels forced wrong labels. Plain wording is now allowed when no label fits.
- A rule for unconfirmed senders downgraded a clear KYC scam (T5). It was narrowed.
- Scam types were not defined, so loan-fee and lottery scams were called "Investment" (T8, T13). Definitions were added in v8.
- N11 (OTP request) failed on v8. See above.

## Prompt versions

| Version | Change |
|---|---|
| v1 | First prompt |
| v2 | Allowed "None found"; rules for genuine and non-message text |
| v3 | Red flags must be real actions; "Can't tell" has scam type None |
| v4 | Flag labels, no repeated quotes, rule for orders aimed at the tool |
| v5 | Call-a-number label; careful wording for "Possibly a scam" |
| v6 | Merged all rules; a too-broad sender rule downgraded T5 |
| v7 | Narrowed that rule; defined pressure |
| v8 | Scam type definitions; no claims about how banks work; 1 to 5 red flags |
| v9 | OTP rule (written, not yet run) |

Check this table against the versions you actually saved. It must match the iteration log in the Milestone 2 PDF and the number in the submission form.

## Limits

- All test messages were written by the team. No real scam messages or user data were used.
- Each message was run once per version. Model output can change between runs.
- The prompt was tuned on these messages, so later passes on them say less than a first pass.
- The tool reads text only. It cannot check a link, phone number, sender or UPI ID, and it cannot read screenshots.
- Only English, Hindi and Hinglish were tested. The Hindi wording was not checked by a native speaker.
- These results are not an accuracy figure. "Looks genuine" does not mean a message is safe.


