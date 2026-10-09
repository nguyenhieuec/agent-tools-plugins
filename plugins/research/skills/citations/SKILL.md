---
name: citations
description: "Paste a bibliography or a single reference and find out whether the papers are real. Use when the user asks things like \"Check my bibliography and tell me which references are real\" or \"Is this DOI correct: 10.1038/nature14539?\"."
license: MIT
---

# Citation Checker

Paste a bibliography or a single reference and find out whether the papers are real. Ask "are these references real?", "check my bibliography", "verify this citation", "is this DOI correct?" or "has this paper been retracted?".

The tools come from the `citations` MCP server, bundled with the `research` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_research_citations__<tool>`, for example `mcp__plugin_research_citations__check_references`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `citations` is connected.

## When to use

- Checks whether the references in a bibliography are real papers
- Flags a DOI that belongs to a different paper than the reference describes
- Finds a wrong year or author list by comparing a reference with the real record
- Reports retractions, expressions of concern and corrections for a paper
- Looks up references that have no DOI by their title, authors and year

## Examples

- "Check my bibliography and tell me which references are real"
- "Is this DOI correct: 10.1038/nature14539?"
- "Has the paper with DOI 10.1016/S0140-6736(97)11096-0 been retracted?"
- "Verify this citation: Hochreiter, S., & Schmidhuber, J. (1997). Long short-term memory. Neural Computation, 9(8), 1735-1780." calls `check_references`.
- "Are these references real? 1) LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. Nature, 521, 436-444. 2) 10.48550/arXiv.1706.03762 3) Smith, J., & Alvarez, M. (2021). Recursive gradient folding in sparse quantum transformers. Journal of Imaginary Results, 12(3), 45-67." calls `check_references`.
- "Is this DOI correct? LeCun, Bengio & Hinton (2015), Deep learning, Nature 521, 436-444, doi:10.1038/nature14540" calls `check_references`.
- "Has this paper been retracted? doi 10.1016/S0140-6736(97)11096-0" calls `check_references`.
- "Check my bibliography: Hochreiter, S., & Schmidhuber, J. (2005). Long short-term memory. Neural Computation, 9(8), 1735-1780." calls `check_references`.

## When not to use

- "Find me five good papers on protein folding that I can cite.": The tool verifies given references against scholarly records; it is not a literature search.
- "Is the conclusion of the 2015 Nature paper Deep learning actually correct?": The plugin compares bibliographic details only; it does not read papers or assess whether their claims are true.
- "Rewrite my bibliography in APA 7 style.": Formatting is a writing task; the tool verifies whether references exist and match, it does not format them.

## Tools

- `check_references`: Check references and DOIs. Use this when the user asks "are these references real?", "check my bibliography", "verify this citation", "is this DOI correct?" or "has this paper been retracted?".

## How to answer

Use check_references when the user wants references, a bibliography, a citation or a DOI checked, or asks whether a paper was retracted. Send each reference as one string as the user wrote it, up to 10 per call; split longer lists into several calls. Report the verdict for every reference. found: a real record matches. mismatch: a real record exists but differences (doi, title, authors, year) say what the reference gets wrong, so show the real record's DOI, title, authors and year. not_found: nothing matches in Crossref or OpenAlex; say it may be invented or garbled, and that books, reports and web pages are often not indexed. unavailable: the lookup failed, suggest trying again. Always state a retraction.status of retracted, partially_retracted, expression_of_concern or corrected, with the notice dates; none_recorded means the source lists no notice, and not_checked means the source carries no retraction data. A year-only mismatch can come from a preprint or a later edition having another year. This checks bibliographic details only; do not claim it verified what a paper says.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
