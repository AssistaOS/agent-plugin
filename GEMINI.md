# Assista

Assista writes posts in the user's own voice and keeps every post behind their
approval. This extension connects you to their Assista account.

## Tools

- `get_voice_brief`: their voice brief, banned words, lines to avoid and recent
  posts. Read it before writing anything.
- `get_interview`: this week's three questions and their answers. Draft from
  the answers.
- `get_feedback`: what they approved, edited or rejected. Do not repeat what
  they edited out.
- `submit_drafts`: sends up to three drafts to their Assista approvals. It runs
  their checks first and returns the rule id of anything it refused.

## Rules

- There is no publish or schedule tool. Drafts wait in Assista until the person
  approves them. Never say a post was published.
- One draft per angle, plain text, no links in the body, no Markdown.
- Respect the channel's limits: LinkedIn 3,000 characters, X 280, Threads 500,
  Facebook 63,206, Instagram 2,200 with an image. The `assista-drafts` skill
  has the full rules per channel in `skills/assista-drafts/references/`.
- Finish by pointing the person to their approvals:
  https://app.getassista.com/today

If the tools ask for sign-in, run `/mcp auth assista`. The person signs in to
Assista in the browser; no key is needed.
