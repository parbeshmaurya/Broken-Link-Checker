---
name: broken-link-checker
description: Check internal and external links from a supplied URL list or website, identify broken links and HTTP errors, and report the broken destination URL, source page, link type, status code, anchor text, and issue in a consolidated CSV.
---

# Broken Link Checker

## Purpose

Identify broken internal and external links from a supplied URL list or website.

For every broken link, report:

- Broken destination URL
- Source / From Page
- Internal or External
- HTTP status
- Anchor text
- Error / Issue

The primary goal is to show **which page contains the broken link and where that link points**.

---

## Required Input

Accept any of the following:

- List of URLs
- CSV file
- XLSX file
- Website/domain for crawling

If a website/domain is provided, crawl accessible pages **within the target domain** and analyze links found on those pages.

Do not crawl external domains. Check external domains only when they are linked from a crawled source page.

If a URL list is provided, check links found on those supplied pages.

---

## Workflow

For each source page:

1. Open the page.
2. Extract links from the HTML.
3. Identify the destination URL for each link.
4. Resolve relative URLs to absolute URLs.
5. Determine whether each link is:
   - Internal
   - External
6. Check the destination URL.
7. Record the HTTP response status.
8. Follow redirects when possible.
9. Identify broken or problematic links.
10. Record the source page containing the link.
11. Record the anchor text when available.
12. Record the issue or error.
13. Consolidate all results into one CSV.

Do not report a link as broken unless the destination was actually checked.

---

## Link Types

### Internal

A link is **Internal** when the destination belongs to the same target domain.

Example:

```text
Source:
https://example.com/services/

Link:
https://example.com/contact/
External

A link is External when the destination belongs to another domain.

Example:

Source:
https://example.com/resources/

Link:
https://anotherwebsite.com/article/

Type:
External
Broken Link Criteria

Classify a link as broken when the destination:

Returns an HTTP 4xx status
Returns an HTTP 5xx status
Has a DNS resolution failure
Fails to establish a connection
Times out and cannot be verified successfully
Produces a redirect loop

Common broken statuses include:

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
410 Gone
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
504 Gateway Timeout

Do not automatically classify these as broken:

301
302
307
308

Follow redirects when possible and check the final destination.

If a redirect eventually reaches a broken destination, report the link as broken and record the final issue.

Redirects

Identify:

Redirected URLs
Redirect chains
Redirect loops
Redirects ending at broken pages

A successful redirect to a working page is not a broken link.

Output
Chat Summary

Keep the chat response concise.

Example:

Broken Link Check Complete

Source Pages Checked: 250
Links Checked: 3,842

Broken Links: 47
Internal Broken Links: 31
External Broken Links: 16

404: 38
5xx Errors: 5
Timeout / Connection Errors: 4

Complete results have been exported to CSV.

Do not display the complete broken-link table in chat.

CSV Output

Generate one consolidated CSV file containing all identified broken links.

Use these columns only:

#	Broken Link	Source / From Page	Link Type	HTTP Status	Anchor Text	Issue
Example
#	Broken Link	Source / From Page	Link Type	HTTP Status	Anchor Text	Issue
1	https://example.com/old-page/	https://example.com/blog/article/	Internal	404	Read more	Page not found
2	https://external.com/resource/	https://example.com/resources/	External	404	Learn more	Page not found
3	https://example.com/file.pdf	https://example.com/downloads/	Internal	500	Download PDF	Server error
4	https://external.com/page/	https://example.com/blog/	External	—	Source	Connection timeout
CSV Rules
Include only links identified as broken or unable to be verified.
Record the exact broken destination URL.
Record the exact source/from page containing the link.
Classify every link as Internal or External.
Record the actual HTTP status when available.
Preserve anchor text when available.
Record a clear issue description.
Do not replace the source page with the domain homepage.
Do not report a link as broken without checking it.
Do not silently skip checked source pages.
Consolidate results from all batches into one CSV.
Duplicate Links

If the same broken URL appears on multiple source pages, report each source-page occurrence.

Example:

Broken URL:
https://example.com/old-page/

Source:
https://example.com/page-a/

Source:
https://example.com/page-b/

Source:
https://example.com/page-c/

These should appear as separate CSV rows because each occurrence requires a separate source-page fix.

If the same broken URL appears multiple times on the same source page, consolidate duplicate occurrences into one row unless the anchor text or link context is materially different.

Bulk Processing

There is no fixed URL limit.

For large URL lists:

Process source pages in manageable batches.
Extract and check links from each batch.
Maintain all source-page relationships.
Consolidate results into one CSV.
Do not create separate CSV files for individual batches.
Do not ask the user to manually split the URL list.

The final CSV must contain all identified broken-link occurrences.

Quality Control

Before delivering the CSV:

Confirm every reported broken link was actually checked.
Confirm every row contains the source/from page.
Confirm every row is classified as Internal or External.
Confirm HTTP status is based on the actual response when available.
Confirm redirects are not incorrectly classified as broken.
Confirm redirect chains and loops are identified.
Confirm duplicate broken links across different source pages are retained.
Confirm all identified broken links are included.
Confirm chat summary totals match the CSV.
Confirm no external domain was crawled as a source domain unless explicitly supplied by the user.
Error Handling
Connection Failure

If the destination cannot be reached after verification attempts, report the connection error in the Issue column.

Timeout

If the destination repeatedly times out and cannot be reliably verified:

Unable to Verify

Record the available HTTP status if one exists; otherwise leave it blank.

DNS Failure

If the domain cannot be resolved:

DNS Resolution Failed
Redirect Loop

If the URL repeatedly redirects without reaching a final destination:

Redirect Loop
Broken Final Destination

If a redirect eventually reaches a 4xx or 5xx page, report the link as broken and record the final HTTP status and issue.

Final Reporting

The final response must contain:

A concise summary in chat.
One consolidated CSV file.

Do not reproduce the complete broken-link dataset in chat.

Only report links that were actually checked and verified as broken or unable to be verified.

Type:
Internal
