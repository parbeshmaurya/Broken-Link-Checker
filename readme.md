Broken Link Checker
What

A Claude Skill that finds broken internal and external links across a website or supplied URL list.

It identifies what link is broken, which page contains it, whether it is internal/external, the HTTP status, and anchor text.

How

**Provide:**
Website/domain or URL list
CSV/XLSX file if available

The skill checks links found on each source page, verifies the destination, follows redirects when possible, and identifies broken URLs, errors, timeouts, and redirect issues.

**Output**
Results are exported to a consolidated CSV:
Broken Link	Source / From Page	Link Type	HTTP Status	Anchor Text	Issue
/old-page/	/blog/article/	Internal	404	Read more	Page not found
external.com/page	/resources/	External	404	Learn more	Page not found

Chat provides only a summary.

**TC — Trust & Control**
Checks the actual destination URL
Distinguishes Internal vs External links
Records the source/from page
Reports actual HTTP status where available
Handles redirects, chains, and loops
Does not report unchecked links as broken
Preserves each source-page occurrence
Consolidates results into one CSV

**Benefits**
Find broken links faster
Identify exactly where fixes are needed
Separate internal SEO issues from external link problems
Support developer/content teams with actionable data
Scale audits across large URL lists
Create client-ready broken-link reports
