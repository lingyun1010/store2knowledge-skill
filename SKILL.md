---
name: storetoknowledge
description: Convert an e-commerce store URL into a structured knowledge base containing products, policies, and FAQs.
---

# Store to Knowledge Base Skill

## Goal

Transform any e-commerce store into a structured knowledge base.

Supported store types:

- Etsy shop
- Shopify store
- Independent e-commerce website
- Other publicly accessible e-commerce pages

## When to Use This Skill

Use this skill when the user wants to turn a store URL into structured knowledge content.

The user may ask things like:

- Convert this store into a knowledge base
- Extract products, policy, and FAQ from this shop
- Generate a product knowledge base from this Etsy store
- Create FAQ and policy documents from this Shopify store
- Analyze this store URL and output markdown, text, or PDF

## Inputs

Required:

- `store_url`: the URL of the store to analyze

Optional:

- `output_format`: `markdown`, `text`, or `pdf`

Default output format:

- `markdown`

## Input Schema

JSON example:

    {
      "store_url": "string",
      "output_format": "markdown | text | pdf"
    }

## Outputs

This skill generates three separate knowledge base files:

1. `products.md` / `products.txt` / `products.pdf`
2. `policy.md` / `policy.txt` / `policy.pdf`
3. `faq.md` / `faq.txt` / `faq.pdf`

Do not combine Products, Policy, and FAQ into a single output file.

## Workflow

### Step 1: Validate the Input

Check that `store_url` is provided.

If `store_url` is missing, return:

    {
      "error": "MISSING_STORE_URL",
      "suggestion": "Please provide a valid e-commerce store URL."
    }

If `output_format` is missing, use:

    markdown

If `output_format` is not one of `markdown`, `text`, or `pdf`, return:

    {
      "error": "INVALID_OUTPUT_FORMAT",
      "allowed_formats": ["markdown", "text", "pdf"],
      "suggestion": "Please choose markdown, text, or pdf."
    }

### Step 2: Access the Store URL

Open the provided `store_url`.

Extract only public information visible on the store website.

Look for:

- Homepage content
- Product listing pages
- Product detail pages
- Shipping policy pages
- Returns policy pages
- Refund policy pages
- Processing time information
- Contact information
- FAQ pages
- About page content

If the store cannot be accessed, follow the Failure Handling section.

### Step 3: Extract Product Information

Extract product information from the store.

For each product, collect:

- Product name
- Product description
- Product category
- Product price, if available
- Product options or variants, if available
- Product usage notes, if available

If a field is missing, write:

    Not specified

Do not invent product names, prices, variants, categories, or benefits.

### Step 4: Extract Policy Information

Extract policy information from the store.

Look for:

- Shipping policy
- Shipping destinations
- Shipping time
- Processing time
- Returns policy
- Refund policy
- Exchange policy
- Cancellation policy
- Contact information

If a field is missing, write:

    Not specified

Do not invent shipping times, refund rules, return windows, or contact details.

### Step 5: Generate FAQ Content

Generate FAQ content using only information found on the store website.

FAQ topics may include:

- What products does the store sell?
- How long does shipping take?
- Does the store accept returns?
- Does the store offer refunds?
- How can customers contact the store?
- How should customers use the products?
- Are there product care instructions?
- Are there product options or variants?

Do not create unsupported answers.

If the answer cannot be found from the store content, write:

    Not specified

### Step 6: Normalize the Knowledge Base

Normalize extracted information into this structure:

    {
      "products": [
        {
          "name": "string",
          "description": "string",
          "category": "string",
          "price": "string",
          "variants": "string",
          "usage_notes": "string"
        }
      ],
      "policy": {
        "shipping": "string",
        "returns": "string",
        "refunds": "string",
        "processing_time": "string",
        "exchanges": "string",
        "cancellations": "string",
        "contact": "string"
      },
      "faq": [
        {
          "question": "string",
          "answer": "string"
        }
      ]
    }

### Step 7: Generate Final Output Files

Generate separate output files according to `output_format`.

Always keep Products, Policy, and FAQ in separate files:

- Products output goes only in `products.[format]`
- Policy output goes only in `policy.[format]`
- FAQ output goes only in `faq.[format]`

The final response should list all generated file names and paths.

## Output Format: Markdown

If `output_format` is `markdown`, create three separate markdown files.

### `products.md`

Use this structure:

    # Products

    Source URL: [store_url]

    | Name | Description | Category | Price | Variants | Usage Notes |
    |---|---|---|---|---|---|
    | Not specified | Not specified | Not specified | Not specified | Not specified | Not specified |

### `policy.md`

Use this structure:

    # Policy

    Source URL: [store_url]

    ## Shipping

    Not specified

    ## Returns

    Not specified

    ## Refunds

    Not specified

    ## Processing Time

    Not specified

    ## Exchanges

    Not specified

    ## Cancellations

    Not specified

    ## Contact

    Not specified

### `faq.md`

Use this structure:

    # FAQ

    Source URL: [store_url]

    ## Q: Not specified

    A: Not specified

## Output Format: Text

If `output_format` is `text`, create three separate text files.

### `products.txt`

Use this structure:

    PRODUCTS

    Source URL:
    [store_url]

    Product 1
    Name: Not specified
    Description: Not specified
    Category: Not specified
    Price: Not specified
    Variants: Not specified
    Usage Notes: Not specified

### `policy.txt`

Use this structure:

    POLICY

    Source URL:
    [store_url]

    Shipping: Not specified
    Returns: Not specified
    Refunds: Not specified
    Processing Time: Not specified
    Exchanges: Not specified
    Cancellations: Not specified
    Contact: Not specified

### `faq.txt`

Use this structure:

    FAQ

    Source URL:
    [store_url]

    Q: Not specified
    A: Not specified

## Output Format: PDF

If `output_format` is `pdf`, first generate the three markdown files:

- `products.md`
- `policy.md`
- `faq.md`

Then export each markdown file as a matching PDF if the runtime supports PDF creation:

- `products.pdf`
- `policy.pdf`
- `faq.pdf`

If PDF creation is not available in the current runtime, return the markdown version and include this note:

    PDF generation is not available in the current runtime. Separate markdown output files have been provided instead.

## Failure Handling

If the store URL cannot be accessed, return:

    {
      "error": "STORE_FETCH_FAILED",
      "store_url": "string",
      "suggestion": "Check the URL or try again later."
    }

If the store is accessible but no product information can be found, return:

    {
      "error": "NO_PRODUCTS_FOUND",
      "store_url": "string",
      "suggestion": "The store may not expose product information publicly, or the products may require JavaScript rendering."
    }

If policy information cannot be found, do not fail the entire task.

Instead, write:

    Not specified

If FAQ information cannot be found, generate FAQ questions only from available product and policy information.

If the answer is not supported by the source content, write:

    Not specified

## Constraints

- Do not hallucinate missing information.
- Do not invent products, prices, variants, shipping times, return rules, refund rules, or contact details.
- Use only public information available from the provided store URL.
- Mark unknown fields as `Not specified`.
- Preserve factual accuracy.
- Keep product and policy information separate.
- Keep Products, Policy, and FAQ in separate output files.
- Do not make medical, legal, financial, or regulated claims.
- Do not scrape private, login-protected, or payment-protected information.
- Do not bypass access controls.
- If the website blocks access, return a structured error instead of guessing.

## Internal Resources

If available, use the following files as supporting prompt templates:

- `prompts/extract_store.md`
- `prompts/generate_products.md`
- `prompts/generate_policy.md`
- `prompts/generate_faq.md`

If available, use the following schemas for normalization:

- `schemas/store_schema.json`
- `schemas/product_schema.json`

If these files are unavailable, continue using the workflow defined in this `SKILL.md` file.

## Quality Checklist

Before returning the final answer, check that:

- The source URL is included.
- Products are listed separately from policies.
- Products, Policy, and FAQ are saved as separate files.
- Unknown fields are marked as `Not specified`.
- No unsupported claims are added.
- The output matches the requested `output_format`.
- The final response lists all generated output files.
