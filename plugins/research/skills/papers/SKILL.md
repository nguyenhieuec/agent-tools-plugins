---
name: papers
description: "Find research papers and arXiv preprints, and get an abstract and free full text from a DOI or id. Use when the user asks things like \"Find recent open-access papers on retrieval-augmented generation evaluation\" or \"Search arXiv for papers on speculative decoding from this year\"."
license: MIT
---

# Paper Finder

Find research papers on a topic, search arXiv preprints, and get a paper's abstract and free full-text link from its DOI, PubMed id or arXiv id.

The tools come from the `papers` MCP server, bundled with the `research` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_research_papers__<tool>`, for example `mcp__plugin_research_papers__search_papers`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `papers` is connected.

## When to use

- Searches Europe PMC and Crossref together for papers on a topic, with an optional start year and an open-access filter
- Searches arXiv preprints by topic, with optional category, start year and newest-first order
- Returns authors, year, venue, DOI, citation count and the index each paper came from
- Shows a free full-text link only when the index confirms one, with its license when stated
- Gets one paper's abstract by DOI, PubMed id or arXiv id
- Names the sources and the date they were read in every answer

## Examples

- "Find recent open-access papers on retrieval-augmented generation evaluation"
- "Search arXiv for papers on speculative decoding from this year"
- "Get the abstract and open-access link for DOI 10.1038/s41586-021-03819-2"
- "Find recent open-access papers on retrieval-augmented generation evaluation." calls `search_papers`.
- "Search arXiv for papers on speculative decoding from this year." calls `search_arxiv`.
- "Get the abstract and open-access link for DOI 10.1038/s41586-021-03819-2." calls `get_paper`.
- "Find systematic reviews on intermittent fasting and cardiovascular outcomes published after 2022." calls `search_papers`.
- "List 5 papers about CRISPR base editing with a PMC full text." calls `search_papers`.

## When not to use

- "Check whether this reference list has fake citations.": Reference, DOI and retraction verification belong to Citation Checker; Paper Finder searches by topic.
- "Summarize this PDF I attached.": The plugin lists papers and abstracts from public indexes; it does not read or summarize files.
- "What medication should I take for my headache?": The plugin finds research papers; it gives no medical advice.

## Tools

- `search_papers`: Search papers. Use this when the user wants papers on a topic, such as "find recent open-access papers on retrieval-augmented generation evaluation" or "systematic reviews on intermittent fasting".
- `search_arxiv`: Search arXiv. Use this when the user wants preprints from arXiv, such as "search arXiv for speculative decoding" or "newest cs.CL papers on tokenization".
- `get_paper`: Get a paper. Use this when the user gives a DOI, PubMed id or arXiv id and wants the paper's abstract or a free full-text link, such as "get the abstract for DOI 10.1038/s41586-021-03819-2".

## How to answer

Use these tools to find research papers and their free full texts. search_papers asks Europe PMC and Crossref about a topic and merges the answers; search_arxiv searches arXiv preprints; get_paper returns one paper's abstract from a DOI, PubMed id or arXiv id. Pass topic words, not a full question. Use from_year for recent work and open_access_only when the user wants papers they can read for free. Report each paper's source and tell the user that openAccessUrl null means the index does not confirm a free copy, not that none exists. Read error.hint when a status is not ok and call again with a changed query instead of repeating the same call. These are records, not endorsements: never present a paper's claims as established fact or give medical advice from an abstract. Do not use these tools to check retractions or verify a reference list: say this plugin does not check them.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
