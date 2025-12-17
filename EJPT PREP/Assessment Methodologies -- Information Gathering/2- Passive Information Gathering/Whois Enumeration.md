**Definition (short):**  
`whois` is a lookup tool that returns domain and IP allocation registration data — owner/registrant, registrar, registration & expiry dates, name servers, and contact/administrative info (when publicly available).

```
whois google.com
```

## Purpose for pentesters (why it’s useful)

1. **Footprinting / Reconnaissance** — reveals ownership hints, related domains, registrars, and name servers that expand the target scope.
    
2. **Infrastructure mapping** — name servers, registrar and registrar glue records can point to hosting providers or shared infrastructure.
    
3. **Correlating assets** — registrant names, email addresses, and phone numbers help link multiple domains to the same organization or threat actor.
    
4. **Finding attack surface** — stale domains, expired domains, or misconfigured name servers can indicate takeover or misconfiguration risks.
    
5. **Social engineering vectors** — contact emails and phone formats suggest real personnel or help craft phishing.
    
6. **Legal / engagement checks** — confirms target ownership and gives registrar contact when you need to request takedowns (always within legal boundaries).
    
7. **Prioritizing targets** — registration dates, domain age, and delegated name servers help estimate how stable or high-value an asset is.
