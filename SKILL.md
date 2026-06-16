# Store to Knowledge Base Skill

## Goal
Transform any e-commerce store (Etsy / Shopify / independent website) into a structured knowledge base.

Input:
- store_url (required)
- output_format: markdown | text | pdf

Outputs:
1. Products
2. Policy
3. FAQ

---

## Input Schema
{
  "store_url": "string",
  "output_format": "markdown | text | pdf"
}

---

## Workflow

Step 1: Extract store data from URL
Step 2: Normalize into structured format
Step 3: Generate outputs

### Products
Structured catalog with name, description, category, price (if available)

### Policy
Shipping, Returns, Refunds, Processing time, Contact

### FAQ
Derived from policies and product usage

---

## Constraints
- Do not hallucinate missing data
- Mark unknown fields as "Not specified"
