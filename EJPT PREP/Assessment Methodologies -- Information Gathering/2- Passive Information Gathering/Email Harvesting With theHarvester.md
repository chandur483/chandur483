**theHarvester** is a passive OSINT tool that helps you quickly collect publicly available email addresses, subdomains/hosts, virtual hosts and sometimes open ports or metadata for a target (company domain, organization name, IP range). It queries search engines, certificate transparency logs, social networks and OSINT services so you can build an initial attack surface without touching the target directly.

# Basic usage (examples a pentester would run)

1. Quick: search a single source (Bing):

```
theHarvester -d target.com -b bing
```

- 2 Search multiple / all supported sources, limit results, save HTML:
```
theHarvester -d target.com -b all -l 500 -f target_report.html
```

`-d` = domain/target, `-b` = source (google, bing, crtsh, all, shodan, hunter, linkedin, etc.), `-l` = limit, `-f` = output filename.

