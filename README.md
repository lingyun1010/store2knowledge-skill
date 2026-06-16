# Store to Knowledge Skill

## What it does
Turn any Shopify / Etsy / independent store into structured knowledge base.

## Input
store_url + output_format

## Output
The skill creates separate files for each knowledge area:

- `products.md` / `products.txt` / `products.pdf`
- `policy.md` / `policy.txt` / `policy.pdf`
- `faq.md` / `faq.txt` / `faq.pdf`

Products, Policy, and FAQ should not be combined into one file.

## Usage
Use in Codex / Claude Code:

Use Store-to-Knowledge Skill
store_url = "https://example.com"
output_format = "markdown"
