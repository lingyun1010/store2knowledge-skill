# Store2Knowledge Skill

Convert any e-commerce store URL into a structured knowledge base.

This skill is designed for Codex / Claude Code style agent workflows. It takes a public store URL, such as an Etsy shop, Shopify store, or independent e-commerce website, and generates structured knowledge content including:

* Products
* Policy
* FAQ

## What This Skill Does

Given a store URL, the skill extracts and organizes public store information into a reusable knowledge base.

It can help generate:

* Product catalog summaries
* Shipping, return, refund, and processing policy summaries
* Customer-facing FAQ content
* Markdown, text, or PDF-style knowledge base output

## Supported Store Types

* Etsy shops
* Shopify stores
* Independent e-commerce websites
* Other publicly accessible store pages

## Repository Structure

```text
store2knowledge-skill/
├── SKILL.md
├── README.md
├── manifest.json
├── skill.yaml
├── prompts/
│   ├── extract_store.md
│   ├── generate_products.md
│   ├── generate_faq.md
│   └── generate_policy.md
├── schemas/
│   ├── store_schema.json
│   └── product_schema.json
└── examples/
    ├── input.json
    └── output_markdown.md
```

## Important File

The main skill entry file is:

```text
SKILL.md
```

For best compatibility with Codex / Claude Code style skill loaders, the file name should be uppercase:

```text
SKILL.md
```

Do not rename it to:

```text
skill.md
Skill.md
```

Some loaders only scan for `SKILL.md`.

## Input

```json
{
  "store_url": "https://example.com",
  "output_format": "markdown"
}
```

## Input Fields

| Field           | Required | Description                  |
| --------------- | -------: | ---------------------------- |
| `store_url`     |      Yes | Public e-commerce store URL  |
| `output_format` |       No | `markdown`, `text`, or `pdf` |

Default output format:

```text
markdown
```

## Output

The skill generates:

```text
Products
Policy
FAQ
```

## Example Use

Ask Codex or Claude Code:

```text
Use the Store2Knowledge skill.

Store URL:
https://example.com

Output format:
markdown
```

Or:

```text
Convert this Etsy shop into a knowledge base:
https://www.etsy.com/shop/example

Return products, policy, and FAQ in markdown.
```

## Natural Language Install

In an environment with GitHub network access, you can ask Codex or Claude Code:

```text
Install this skill from GitHub:
https://github.com/lingyun1010/store2knowledge-skill
```

Then use:

```text
Use the store_to_knowledge skill on this store URL:
https://example.com
```

## If Natural Language Install Fails

Some Codex / Claude Code sessions run inside a sandbox where outbound internet access is disabled.

If installation fails with an error like:

```text
Could not resolve host: github.com
```

or:

```text
I don't have working network access to fetch the repo
```

then the problem is the current session environment, not this repository.

This usually means:

* The session cannot access GitHub
* DNS/network access is disabled
* The session does not expose a network approval flow
* The agent cannot run `git clone` from inside the sandbox

## How to Check Network Access

Ask Codex to run:

```bash
curl -I https://github.com
```

If it returns HTTP headers, GitHub is reachable.

If it returns:

```text
Could not resolve host: github.com
```

then the current session cannot access GitHub.

## What to Do If GitHub Access Is Blocked

### Option 1: Open a New Session

Sometimes a new Codex session has a different sandbox or permission profile.

Start a new session and ask:

```text
Before installing anything, test network access:

curl -I https://github.com

If GitHub is reachable, install:
https://github.com/lingyun1010/store2knowledge-skill
```

### Option 2: Use a Session With Network Approval

If the environment supports approval requests, ask:

```text
Please request network access approval so you can fetch GitHub repositories.
Then install:
https://github.com/lingyun1010/store2knowledge-skill
```

If no approval prompt appears, the current environment probably does not support network approval.

### Option 3: Install Manually With Codex CLI

If you are using local Codex CLI, install the skill manually:

```bash
mkdir -p ~/.codex/skills

git clone https://github.com/lingyun1010/store2knowledge-skill \
~/.codex/skills/store2knowledge-skill
```

Then restart Codex CLI.

### Option 4: Copy the Skill Locally

If you already downloaded this repository:

```bash
mkdir -p ~/.codex/skills/store2knowledge-skill

cp -R /path/to/store2knowledge-skill/* \
~/.codex/skills/store2knowledge-skill/
```

Replace `/path/to/store2knowledge-skill` with the actual local path.

## Codex App Notes

If you are using Codex App instead of Codex CLI, the app session may not expose internet/network settings.

In that case:

* Natural language GitHub install may fail
* The app session may not be able to request network approval
* Opening a new session may help if the new session exposes a different permission flow
* If GitHub access remains blocked, use upload/copy/manual installation instead

The key error is:

```text
Could not resolve host: github.com
```

This means the session cannot reach GitHub from inside its sandbox.

## Troubleshooting

### Problem: `Could not resolve host: github.com`

Cause:

The current session has no outbound network access.

Fix:

* Open a new session and test GitHub access first
* Use a session/environment with network access enabled
* Use local CLI manual install
* Copy the skill files manually

### Problem: Skill is not detected

Check that the repo root contains:

```text
SKILL.md
```

The file should be uppercase.

Also check that `manifest.json` points to:

```json
{
  "entry": "SKILL.md"
}
```

### Problem: Installer asks for a path

Use:

```text
SKILL.md
```

or, if installing manually, place the repo at:

```text
~/.codex/skills/store2knowledge-skill
```

### Problem: PDF output is not generated

Some runtimes do not support PDF creation.

If PDF export is unavailable, the skill should return markdown output and explain that PDF generation is not available in the current runtime.

## Recommended First Prompt After Install

```text
Use the store_to_knowledge skill.

Store URL:
https://example.com

Output format:
markdown

Generate:
1. Products
2. Policy
3. FAQ

Do not hallucinate missing information.
Mark unknown fields as "Not specified".
```

## Design Principles

This skill follows these rules:

* Do not hallucinate missing information
* Use only public store information
* Do not invent products, prices, policies, or FAQs
* Mark unknown fields as `Not specified`
* Keep products, policy, and FAQ clearly separated
* Do not bypass access controls
* Do not scrape private or login-protected information

## License

MIT
