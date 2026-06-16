# Store2Knowledge Skill

Convert a public e-commerce store URL into three structured knowledge files:

- `products.md`
- `policy.md`
- `faq.md`

The skill is designed for Codex / Claude Code style agent workflows. It works with Shopify stores, Etsy shops, independent e-commerce sites, and other publicly accessible store pages.

## What It Does

Given a store URL, the skill extracts public information and organizes it into:

- Product catalog summaries
- Shipping, return, refund, processing, exchange, cancellation, and contact policy details
- Customer-facing FAQ content based only on extracted product and policy information

Unknown fields are marked as `Not specified`.

## Repository Structure

```text
store2knowledge-skill/
├── SKILL.md
├── README.md
├── manifest.json
├── skill.yaml
└── prompts/
    ├── extract_store.md
    ├── generate_products.md
    ├── generate_policy.md
    └── generate_faq.md
```

`SKILL.md` is the main entry file. Keep the filename uppercase because some skill loaders only scan for `SKILL.md`.

## Input

```json
{
  "store_url": "https://example.com",
  "output_format": "markdown"
}
```

| Field | Required | Description |
|---|---:|---|
| `store_url` | Yes | Public e-commerce store URL |
| `output_format` | No | `markdown`, `text`, or `pdf`; defaults to `markdown` |

## Output

The skill must keep Products, Policy, and FAQ in separate files.

For `markdown` output:

```text
products.md
policy.md
faq.md
```

For `text` output:

```text
products.txt
policy.txt
faq.txt
```

For `pdf` output, the skill first generates separate markdown files, then exports matching PDFs if the runtime supports PDF creation:

```text
products.pdf
policy.pdf
faq.pdf
```

If PDF export is unavailable, the skill should return the separate markdown files and explain that PDF generation is not available in the current runtime.

## Usage

```text
Use the Store2Knowledge skill.

store_url = "https://example.com"
output_format = "markdown"
```

Expected result:

```text
products.md
policy.md
faq.md
```

## Install

Ask Codex or Claude Code:

```text
Install this skill from GitHub:
https://github.com/lingyun1010/store2knowledge-skill
```

If installing manually:

```bash
mkdir -p ~/.codex/skills/store2knowledge
git clone https://github.com/lingyun1010/store2knowledge-skill ~/.codex/skills/store2knowledge
```

Restart Codex after installing.

## Troubleshooting

### GitHub Cannot Be Reached

Check network access:

```bash
curl -I https://github.com
```

If you see `Could not resolve host: github.com`, the current session cannot access GitHub. Use a session with network access, request network approval if available, or install the skill manually from a local copy.

### Skill Is Not Detected

Check that the installed skill folder contains:

```text
SKILL.md
```

Also check that `manifest.json` points to:

```json
{
  "entry": "SKILL.md"
}
```

## Design Principles

- Use only public store information
- Do not hallucinate missing information
- Do not invent products, prices, variants, policies, shipping times, refund rules, or contact details
- Mark unknown fields as `Not specified`
- Keep Products, Policy, and FAQ in separate output files
- Do not scrape private, login-protected, or payment-protected information
- Do not bypass access controls

## License

MIT
