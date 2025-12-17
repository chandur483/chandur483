### What are Google Dorks?

**Google Dorks** (also called _Google Dorking_ or _Google Hacking_) is a technique where you use **advanced search operators** in Google to find information that isn’t meant to be public but is accidentally exposed online.

It’s basically turning Google into a **powerful recon tool**.

### How it Works

Google has special operators like:

- `site:` → search within a specific website.
    
- `filetype:` → search for certain file types (e.g., PDF, DOCX, SQL).
    
- `intitle:` → find pages with specific words in the title.
    
- `inurl:` → look for keywords in the URL.
    
- `cache:` → view Google’s cached version of a page.


_________

# Google Dorks — Cheat Sheet (Defensive & Pentest)

Compact, printable cheat-sheet of Google search operators ("Google dorks") with quick examples, defensive uses, and ready-to-run queries. **Only run these on assets you own or are authorized to test.**

---

## Quick operator table

|Operator|What it does|Example|Defensive / pentest use|
|---|--:|---|---|
|`site:`|Restrict search to a domain, subdomain or path|`site:example.com`|Check what of your domain is indexed|
|`-site:`|Exclude a domain|`filetype:pdf -site:example.com`|Remove noisy sources when hunting leaks|
|`filetype:`|Find files by extension (pdf, xls, sql)|`site:example.com filetype:sql`|Find exposed backups/config dumps|
|`ext:`|Alias for `filetype:` (occasionally used)|`ext:env site:example.com`|Alternate extension search|
|`intitle:`|Word appears in page title|`intitle:"admin panel"`|Find likely admin pages|
|`allintitle:`|All following words must be in title|`allintitle: admin login`|Narrow title-based results|
|`inurl:`|Word appears in the URL|`inurl:wp-admin`|Find CMS admin paths or dev endpoints|
|`allinurl:`|All following words must be in URL|`allinurl: admin login`|Precise URL filtering|
|`intext:`|Word appears in page body text|`intext:"DB_PASSWORD"`|Locate pages containing sensitive keywords|
|`allintext:`|All words must be in the page text|`allintext: confidential password`|Narrow content searches|
|`inanchor:`|Word appears in anchor (link) text|`inanchor:"download"`|Find linked file pages or mirrors|
|`allinanchor:`|All words in anchor text|`allinanchor: user manual`|Focused anchor-based lookups|
|`cache:`|View Google’s cached version of a page|`cache:example.com/index.php`|Inspect removed content still indexed|
|`info:`|Google info page for a URL|`info:example.com`|Quick metadata about a URL in Google|
|`related:`|Find sites similar to the specified URL|`related:example.com`|Map similar/clone sites or mirrors|
|`link:`|Pages that link to a URL (deprecated/unreliable)|`link:example.com`|Historical backlink checks (use cautiously)|
|`AROUND(n)`|Find terms within _n_ words of each other|`"error" AROUND(5) "database"`|Contextual matching for phrases|
|`before:` / `after:`|Date filters (`YYYY-MM-DD` or `YYYY`)|`site:example.com after:2023-01-01`|Timeline of indexed pages/exposures|
|`OR`|Logical OR (uppercase)|`filetype:pdf OR filetype:docx`|Broaden file-type searches|
|`-` (minus)|Exclude terms|`site:example.com -inurl:blog`|Filter out noisy subpaths|
|`""` (quotes)|Exact phrase match|`"index of /backup"`|Pinpoint exact strings (e.g., credentials)|
|`*` (wildcard)|Placeholder inside quotes|`"password * file"`|Search variations of phrases|

---

## High-value example queries (defensive / pentest)

**Find exposed documents**

```
site:yourdomain.com (filetype:pdf OR filetype:docx OR filetype:xlsx OR filetype:csv)
```

**Find backups / DB dumps**

```
site:yourdomain.com (filetype:sql OR filetype:bak OR inurl:backup OR inurl:dump)
```

**Search for credential-like strings**

```
site:yourdomain.com intext:"password" OR intext:"DB_PASSWORD" OR intext:"passwd"
```

**Find directory listings**

```
site:yourdomain.com intitle:"index of" OR inurl:"/index.of/"
```

**Locate admin/login pages**

```
site:yourdomain.com (intitle:login OR inurl:login OR inurl:signin OR inurl:wp-admin)
```

**Find config files / backups accidentally exposed**

```
site:yourdomain.com "wp-config.php" OR filetype:env OR filetype:ini OR filetype:conf
```

---

## HackerSploit-focused examples (educational; scope `site:hackersploit.org`)

Use these to learn how dorks match real world results — **do not run against third-party sites without permission**.

- PDFs and resources on HackerSploit:
    

```
site:hackersploit.org filetype:pdf
```

- Forum uploads / attachments:
    

```
site:hackersploit.org inurl:forum OR inurl:uploads filetype:pdf
```

- Search for index listings on hackersploit:
    

```
site:hackersploit.org intitle:"index of"
```

- Look for admin/login endpoints (publicly reachable login pages):
    

```
site:hackersploit.org inurl:admin OR inurl:wp-admin
```

- Search for credential-like text (to understand risk patterns):
    

```
site:hackersploit.org intext:"password" OR intext:"DB_PASSWORD"
```

---

## Defensive quick-check checklist (run on _your_ domain)

1. `site:yourdomain.com filetype:pdf OR filetype:xlsx OR filetype:docx` — remove or secure leaked docs.
    
2. `site:yourdomain.com intext:"password" OR intext:"DB_PASSWORD"` — rotate any exposed secrets.
    
3. `site:yourdomain.com intitle:"index of"` — disable directory indexing.
    
4. `site:yourdomain.com inurl:backup OR inurl:dump OR filetype:sql` — remove public backups.
    
5. `site:yourdomain.com inurl:admin OR inurl:wp-admin` — protect login pages (WAF, MFA, IP restrictions).
    
6. Add `noindex` or `X-Robots-Tag: noindex` to non-public pages and remove sensitive files from public webroot.
    

---

## Responsible disclosure & ethics

- Never use dorks to access private data on systems you do not own or are not authorized to test.
    
- If you accidentally find sensitive data on a third-party site, follow responsible disclosure: contact the site owner, CERT, or the appropriate disclosure channel.
    
- Use dorks primarily for defensive discovery, research, and authorized penetration testing.
    

---

## Printable variants & exports

If you want this as a **one-page PDF**, **CSV** (table only), or a compact **10-high-value-dorks cheat-sheet**, tell me which format and I will export it for you.

---

_Made for defensive use and learning — keep it legal and ethical._