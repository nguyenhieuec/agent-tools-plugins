---
name: eu-business
description: "Validate an EU VAT number on VIES, look up an LEI, or search a company register. Use when the user asks things like \"Which company is behind the domain doctolib.fr, and how big is it?\" or \"Is the Austrian VAT number ATU33864707 valid, and who is it registered to?\"."
license: MIT
---

# EU Business & VAT Checker

Validate an EU VAT number on VIES, look up an LEI, or search a company register. Ask "is ES A28015865 a valid VAT number?", "what is the LEI of Siemens AG?" or "find the registered company called Equinor in Norway".

The tools come from the `eu-business` MCP server, bundled with the `office` plugin.
In Claude Code they are named `mcp__plugin_office_eu-business__<tool>`, for example `mcp__plugin_office_eu-business__company_from_domain`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `eu-business` is connected.

## When to use

- Returns the legal name, country, registration number, size band and age behind a company domain, each with a source link
- Validates an EU or Northern Ireland VAT number on VIES and shows the name of a legal entity
- Issues a VIES consultation number when given the requester's own VAT number
- Finds the Legal Entity Identifier (LEI) of a company by name, or looks a LEI up
- Searches the French Sirene register by SIREN and the Norwegian company register by name or number
- Leaves out sole proprietors and withholds names that could be a private person

## Examples

- "Which company is behind the domain doctolib.fr, and how big is it?"
- "Is the Austrian VAT number ATU33864707 valid, and who is it registered to?"
- "What is the LEI of Siemens Aktiengesellschaft in Germany?"
- "Which company is behind the domain doctolib.fr, and how big is it?" calls `company_from_domain`.
- "Is the Austrian VAT number ATU33864707 valid, and who is it registered to?" calls `check_vat_number`.
- "Check the Spanish VAT number ESA28015865 for me." calls `check_vat_number`.
- "What is the LEI of Siemens Aktiengesellschaft in Germany, and is it active?" calls `find_company_lei`.
- "Find the French company Danone in the company register and tell me where its head office is." calls `lookup_company_registry`.

## When not to use

- "Who owns acme.com and what is the CEO's name and email?": The tool returns company facts only. It never returns a person, an email address or a phone number.
- "Is this supplier financially stable and safe to give 90 days credit?": A VAT or register entry says a business exists, not that it is solvent or trustworthy.
- "What is the standard VAT rate in Poland, and must I register there?": The plugin checks VAT numbers, not VAT rates or registration limits.

## Tools

- `company_from_domain`: Find the company behind a domain. Use this when the user has a company domain, a website or a work email address and wants to know the company behind it, for example "which company is behind doctolib.fr?", "how big and how old is the company at stripe.com?" or "who is the legal entity for the lead that wrote from jane@acme.io?".
- `check_vat_number`: Validate an EU VAT number on VIES. Use this when the user asks to validate a VAT number, check a tax ID, or confirm a supplier number for a reverse-charge invoice, for example "is ES A28015865 a valid VAT number?", "check the supplier VAT number on this invoice, ATU33864707" or "I need a VIES consultation number for this supplier".
- `find_company_lei`: Look up a Legal Entity Identifier. Use this when the user asks for the Legal Entity Identifier (LEI) of a company, including a KYB or vendor onboarding check, for example "what is the LEI of Siemens AG?", "whose LEI is W38RGI023J3WT1HWRP32?" or "is the LEI of this German company still active?".
- `lookup_company_registry`: Search a French SIREN or Norwegian register. Use this when the user asks about a company registered in France or Norway, or for the company registration number on a Kbis extract, for example "find the French company Danone", "who is behind SIREN 552032534?" or "look up Equinor in the Norwegian business register".

## How to answer

Use company_from_domain when the user has a domain, website or work email and wants the company behind it. Pass the domain, such as stripe.com. Give each fact with its source link and date. Explain confidence: confirmed means a register checked the number or Wikidata link, name_match means only the name matched one register entry, printed_on_site means only the company's own page says it, wikidata_only means only Wikidata says it. Never fill a gap: say a field in unknown is unknown and why. If status is ambiguous, show the candidates and ask; if it is not_a_company_domain, give the reason. Domain age is not company age. A size band can be years old and covers one legal entity, not its group. Use check_vat_number to validate an EU or Northern Ireland VAT number, find_company_lei for a Legal Entity Identifier, and lookup_company_registry for the French or Norwegian register (other countries: use find_company_lei or say the register is not covered). Send the VAT country as a two-letter code (Greece is EL, Northern Ireland is XI; GB cannot be checked); ask for it if the user gave only digits. Say the result is what the official source reports on that date. A valid number does not show the business is trustworthy or is the one the user deals with: suggest comparing the registered name with the invoice. Send requesterCountry and requesterVatNumber only when the user gives their own VAT number for proof of the check, and pass on the consultationNumber. Pass on the register lag notice. Companies only: if a name is withheld, say so and do not guess it. Never identify or search for a private person, owner, director or employee. This server does not answer VAT registration duties or VAT rates.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
