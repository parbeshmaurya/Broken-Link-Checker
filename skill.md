---
name: broken-link-checker
description: Crawl a website or supplied URL list using direct browser/web access, check internal and external links, identify broken destinations, and export the source page, broken URL, link type, HTTP status, anchor text, and issue to a consolidated CSV.
---

# Broken Link Checker

## Purpose

Check broken internal and external links directly from a website or supplied URL list.

For every broken link, identify:

- Broken destination URL
- Source / From Page
- Link Type — Internal or External
- HTTP Status
- Anchor Text
- Issue

The main objective is to show **where the broken link exists and what destination it points to**.

---

## Required Input

Accept:

- Website/domain
- List of URLs
- CSV file
- XLSX file

### Website / Domain

When a website/domain is provided:

1. Open the supplied website.
2. Discover internal links from accessible pages.
3. Recursively discover additional internal URLs.
4. Process discovered accessible pages.
5. Continue until no new accessible internal pages within the crawl scope remain.
6. Check links found on those pages.

Do not rely on Ahrefs, Semrush, Google Search Console, or another third-party SEO crawler.

### URL List

When a URL list is provided:

- Check every supplied URL as a source page.
- Extract links from each page.
- Check every discovered destination.
- Preserve the source-page relationship.

---

## Direct Browser/Web Checking

The skill must use direct access to the actual website and URLs.

Do not substitute:

- Ahrefs
- Semrush
- Google Search Console
- Screaming Frog
- Third-party SEO audit reports
- Historical crawl data

The skill should report only information obtained from URLs it actually accesses and checks.

---

## Website Crawling

For a domain audit:

1. Start with the supplied domain.
2. Open the page.
3. Extract links from the accessible HTML.
4. Resolve relative URLs to absolute URLs.
5. Identify internal URLs.
6. Add new internal URLs to the crawl queue.
7. Visit each accessible internal URL.
8. Repeat the process until no new accessible internal URLs remain within the crawl scope.
9. Check destination URLs for discovered links.
10. Record broken-link occurrences.

Do not crawl external domains as source pages.

External domains should only be checked as destinations linked from the target website.

---

## Crawl Coverage

The skill must distinguish between:

### Full Crawl

All discoverable and accessible internal pages within the crawl scope were processed.

Report:

```text
Crawl Type: Full Crawl

Partial Crawl

Some pages could not be accessed, discovered, or processed.

Report:

Crawl Type: Partial Crawl

Also report:

Pages discovered
Pages successfully checked
Pages unavailable
Links checked

Never claim a full crawl if only a sample or partial set of pages was checked.

Do not silently switch to manual page checking and describe it as a full-site crawl.

Link Extraction

For every accessible source page:

Extract links from the page.
Resolve relative URLs.
Remove URL fragments when checking the destination.
Identify the destination URL.
Identify anchor text when available.
Classify the link as Internal or External.
Check the destination.

Include links such as:

HTML links
Navigation links
Content links
Footer links
Image links
PDF links
Document links

Ignore non-navigation JavaScript actions that do not contain a real destination URL.

Link Types
Internal

A link is Internal when the destination belongs to the target domain.

Example:

Source:
https://example.com/services/

Destination:
https://example.com/contact/

Type:
Internal
External

A link is External when the destination belongs to another domain.

Example:

Source:
https://example.com/resources/

Destination:
https://anotherwebsite.com/article/

Type:
External
Broken Link Criteria

Classify a destination as broken when it:

Returns HTTP 4xx
Returns HTTP 5xx
Has a DNS resolution failure
Cannot establish a connection
Repeatedly times out
Produces a redirect loop
Redirects to a broken destination

Common broken statuses:

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

A successful redirect to a working page is not a broken link.

Redirect Checking

Identify:

Redirected URLs
Redirect chains
Redirect loops
Redirects ending at broken pages

If a URL redirects to a working final destination, do not report it as broken.

If a URL eventually redirects to a 4xx or 5xx destination, report it as broken.

Source Page Requirement

Every broken-link result must include the exact page where the link was found.

Do not report only:

https://example.com/old-page/

Report:

Broken Link:
https://example.com/old-page/

Source / From Page:
https://example.com/blog/article/

The source page must always be retained.

Duplicate Links

If the same broken URL appears on multiple source pages, record every source-page occurrence.

Example:

Broken URL:
https://example.com/old-page/

Source:
https://example.com/page-a/

Source:
https://example.com/page-b/

Source:
https://example.com/page-c/

These should be separate CSV rows.

If the same broken URL appears multiple times on the same source page, consolidate it into one row unless the anchor text or link context is materially different.

Output
Chat Summary

Show only a concise summary in chat.

Do not display the complete broken-link dataset.

Example:

Broken Link Check Complete

Crawl Type: Full Crawl
Source Pages Checked: 1,248
Links Checked: 18,642

Broken Links: 127
Internal: 89
External: 38

404: 103
5xx: 8
DNS / Connection: 9
Timeout: 5
Redirect Issues: 2

Complete results have been exported to CSV.

For a partial crawl:

Broken Link Check Incomplete

Crawl Type: Partial Crawl
Pages Discovered: 1,248
Pages Checked: 842
Pages Unable to Check: 406
Links Checked: 12,315

Broken Links Found: 83

The CSV contains results from the pages successfully checked.

Do not call a partial crawl "Complete."

CSV Output

Generate one consolidated CSV file containing all identified broken-link occurrences.

Use these columns only:

#	Broken Link	Source / From Page	Link Type	HTTP Status	Anchor Text	Issue
Example
#	Broken Link	Source / From Page	Link Type	HTTP Status	Anchor Text	Issue
1	https://example.com/old-page/	https://example.com/blog/article/	Internal	404	Read more	Page not found
2	https://external.com/resource/	https://example.com/resources/	External	404	Learn more	Page not found
3	https://example.com/file.pdf	https://example.com/downloads/	Internal	500	Download PDF	Server error
4	https://external.com/page/	https://example.com/blog/	External	—	Source	Connection timeout
CSV Rules
Generate one consolidated CSV.
Include every verified broken-link occurrence.
Record the exact broken destination URL.
Record the exact source/from page.
Classify each result as Internal or External.
Record the actual HTTP status when available.
Preserve anchor text when available.
Record a clear issue description.
Do not replace the source page with the homepage.
Do not report unchecked URLs as broken.
Do not silently skip accessible source pages.
Preserve separate source-page occurrences.
Error Handling
404 / 410

Record the actual HTTP status.

HTTP Status: 404
Issue: Page not found
5xx

Record the actual server status.

HTTP Status: 500
Issue: Internal server error
DNS Failure
HTTP Status: —
Issue: DNS Resolution Failed
Connection Failure
HTTP Status: —
Issue: Connection Failed
Timeout

If the destination cannot be reliably verified:

HTTP Status: —
Issue: Unable to Verify - Connection Timeout
Redirect Loop
HTTP Status: —
Issue: Redirect Loop
Bulk Processing

There is no fixed URL limit.

For large websites or URL lists:

Process pages in manageable batches.
Maintain the source-page relationship.
Continue processing all accessible pages.
Check all discovered destinations.
Consolidate all results into one CSV.
Do not create separate CSV files for individual batches.
Do not ask the user to manually split the URL list.
Quality Control

Before delivering the results:

Confirm every reported broken link was actually checked.
Confirm every result contains the source/from page.
Confirm every result is classified as Internal or External.
Confirm HTTP status is based on the actual response when available.
Confirm redirects were evaluated before classifying a link as broken.
Confirm redirect chains and loops are identified.
Confirm duplicate broken URLs across different source pages are retained.
Confirm all accessible source pages within the crawl scope were processed.
Confirm crawl coverage is correctly classified as Full or Partial.
Confirm chat summary totals match the verified results.
Confirm no third-party SEO crawler data was substituted.
Never claim a full-site crawl when only part of the site was checked.
Tool Restrictions

Do not use third-party SEO platforms as a substitute for direct URL checking.

Do not use:

Ahrefs
Semrush
Google Search Console
Screaming Frog
Third-party SEO audit reports
Historical crawl data

The skill must rely on direct browser/web access to the actual source and destination URLs.

If direct access is unavailable, report the limitation accurately.

Do not invent, estimate, or infer broken-link results.

Final Reporting

The final response must contain:

A concise summary in chat.
One consolidated CSV file.

Do not reproduce the complete broken-link dataset in chat.

Only report links that were actually checked and verified as broken or unable to be verified.

Always state whether the audit was:

Full Crawl
Partial Crawl
URL List Check
Unable to Verify

The results represent the URLs accessible and checked at the time of the audit.


### Key change

The most important instruction is now:

> **Do not silently switch to manual page checking and describe it as a full-site crawl.**

So if Claude can access 1,000 pages, it should crawl/process those pages directly. If it can only access 100, the output must say **Partial Crawl — 100 pages checked**, rather than pretending it audited the whole site.

And **no Ahrefs/Semrush dependency is required**.
